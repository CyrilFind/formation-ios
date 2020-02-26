# TP 3: Swift UI - List

## Création

- Créer une app "Master Detail" en choississant SwiftUI
- Lancer l'app pour tester un peu
- Créer une struct `Task`:

```swift
struct Task {
    var id: String
    var title: String
    var description: String

    init(id: String? = nil, title: String = "", description: String = "") {
        self.id = id ?? UUID().uuidString
        self.title = title
        self.description = description
    }
}

```

- Changez le fonctionnement de l'app pour qu'elle affiche des `Task` et pas des `Date`:
  - Afficher le `title` seulement dans la liste (`MasterView`)
  - Afficher le `title` en tant que titre de la `DetailView`
  - Afficher la description comme au milieu de la `DetailView`
- Changez la `DetailView` pour que les infos de la `Task` soient éditables:

```swift
Form {
    TextField("Title", text: $task.title)
    TextField("Description", text: $task.description)
}
```

- Utilisez les `@Binding` et les `$variable` pour passer la donnée à travers vos vues
- Vous pouvez ajouter une méthode `titleOrEmpty()` à `Task` pour éviter un affichage vide dans la liste et dans le titre de la vue détail:

```swift
func titleOrEmpty() -> String {
    return title.isEmpty ? "No Title" : title
}
```

Vous aurez ainsi une liste avec Ajout et Édition 👏

## Internet

- Ajoutez Alamofire
