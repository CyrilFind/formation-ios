# TP 3: SwiftUI List

## Création

- Créer une app "Master-Detail" en choisissant "SwiftUI"
- Lancer l'app pour tester un peu

## Adaptation

- Créer une `struct` `Task`:

```swift
struct Task : Identifiable {
  var id: String?
  var title: String
  var description: String

  init(id: String? = nil, title: String = "", description: String = "") {
    self.id = id
    self.title = title
    self.description = description
  }
}
```

Vous allez changer le fonctionnement de l'app pour qu'elle affiche des `Task` et pas des `Date` (pensez à lancer l'app au fur et à mesure)

### DetailView

- remplacez `selectedDate` par `@Binding var task: Task`
- Modifiez l'appel de `DetailView()` en conséquence
- Afficher le `title` en tant que titre
- Afficher la description dans le `Text` au milieu
- Remplacez le `Group` pour que les infos de la `Task` soient éditables:

```swift
Form {
  TextField("Title", text: $task.title)
  TextField("Description", text: $task.description)
}
```

### MasterView

- Afficher le `title` seulement dans la liste
- Créez une classe `ContentViewModel`:

```swift
class ContentViewModel : ObservableObject {
  @Published var tasks = [Task]()

  func addTask() {
      self.tasks.append(Task())
  }

  func removeTask(_ task: Task) {
    self.tasks.removeAll(where: { $0.id == task.id })
  }
}
```

- Remplacez `dates` par un `@ObservedObject`:

```swift
    @ObservedObject var viewModel = ContentViewModel()
```

- Utilisez cette propriété et `addTask` dans l'action du bouton "+"
- Utilisez cette propriété et `removeTask` dans `onDelete`

- Ajoutez une méthode `titleOrEmpty()` à `Task` pour éviter un affichage vide dans la liste et dans le titre de la vue détail:

```swift
func titleOrEmpty() -> String {
  return title.isEmpty ? "No Title" : title
}
```

> Vous aurez ainsi une liste avec Ajout, Édition et Suppression en local 👏

## Internet

- Ajoutez Alamofire: Fichier de configuration (icône bleue) > Project > même icône > Swift Packages > coller <https://github.com/Alamofire/Alamofire.git>)

- Ajouter ceci à la `struct` `Task` et la faire hériter également de `Codable` afin qu'elle soit facilement "parsable"

```swift
enum CodingKeys: String, CodingKey {
 case id = "id"
 case title = "title"
 case description = "description"
}
```

- Récupérez un token sur l'api: <https://android-tasks-api.herokuapp.com/api>
- Créer une classe `Api`:

```swift
class Api {
    private static let TOKEN = VOTRE_TOKEN_ICI
    private static let BASE_API = "https://android-tasks-api.herokuapp.com/api"

    static func getTasks(completion: @escaping ([Task]) -> Void ) {
        AF.request("\(BASE_API)/tasks",
            headers: ["Authorization": "Bearer \(Api.TOKEN)"]
        ).responseDecodable(of: [Task].self) { (response) in
            guard let tasks = response.value else { return }
            completion(tasks)
        }
    }
```

- Ajoutez  `ContentViewModel`:

```swift
func refresh() {
  Api.getTasks { fetchedTasks in
    self.tasks = fetchedTasks
  }
}
```

- Modifiez `ContentView` pour qu'il récupère les tâches de l'API au lancement en ajoutant ceci:

```swift
.onAppear {
  self.viewModel.refresh()
}
```

- Ajoutez un `Button` dans `ContentView` > `.navigationBarItems` > `trailing:` (avec une `HStack`):

```swift
Button(action: {
  withAnimation {
    self.viewModel.refresh()
  }
}) {
  Image(systemName: "arrow.2.circlepath")
}
```

- Inspirez vous de cela pour faire de même pour l'ajout, la suppression et l'édition

> Indices (arguments de `request`):

- ajout: `..., parameters: task, encoder: JSONParameterEncoder.default, ...`
- suppression: `"\(BASE_API)/tasks/\(id)", method: .delete, ...`
- édition: comme l'ajout
