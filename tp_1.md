# TP 1: UI Kit

## Création

- Créer une "Single View App" nommée "Bull's Eyes UIKit" avec l'option "StoryBoard"
- Lancez la ▶
- Ouvrez Main.Storyboard
- Choisir dans la visualisation le même iPhone que dans le Simulator, passer en paysage.
- Cliquer sur le bouton  (un carré dans un rond) pour ajouter un bouton
- Changez son texte: `"Hit Me !"`
- Dans ViewController, ajouter:

```swift
@IBAction func showAlert() {
  print("Hello")
}
```

- Dans le storyboard, appuyez sur `control` et cliquez sur le bouton puis glissez la souris jusqu'à l'icône jaune juste au dessus (ou dans le volet de gauche: "View Controller")
- Dans la popoup qui va s'ouvrir, cliquer sur `showAlert()`: c'est la méthode que l'on vient de créer
- Dans le volet de droite, dans le dernier onglet  (une flèche dans un cercle), vérifiez que "Touch up Inside" est bien relié à `showAlert()`
- Lancez l'app et vérifiez que la console affiche bien "Hello !"

## Interaction

- Dans `showAlert()`, retirez le `print` et créez une Alert (popup):

```swift
let alert = UIAlertController(title: "Hello World!", message: "This is my first App!", prefferedStyle: .alert)
```

- Créez une action

```swift
let action = UIAlertAction(title: "Awesome", style: .default, handler: nil)
```

- Ajoutez cette action à votre alert

```swift
alert.addAction(action)
```

- Affichez l'alert:

```swift
present(alert, animated: true, completion: nil)
```

## UI

- Allez dans les settings du projet: icone bleue tout en haut de l'arborescence
- Dans les orientations, autoriser seulement "landscape left" et "landscape right"
- Ajouter tous les éléments de base de l'interface: labels, sliders, buttons:
- Pour le bouton info, setter le "type" sur "Info Light"
- Pour le slider, modifier les valeurs de départ, d'arrivée et actuelles sur: 1, 100, 50

## Gameplay

- Ajouter une méthode `sliderMoved()`:

```swift
@IBAction func sliderMoved(_ slider: UISlider) {
    print("The slider value is \(slider.value)")
}
```

- Relier le slider à cette méthode: cette fois c'est "value changed" qui va être relié
- Créez une propriété:

```swift
val currentValue: Int = 0
```

- Pour stocker dedans la valeur récupérée dans sliderMoved, vous devrez faire un *cast*:

```swift
intValue = Int(floatValue)
```

- Changez le message de l'alert pour afficher le résultat
- Arrondissez la valeur (le cast fait une troncature par défaut):

```swift
let roundedValue = floatValue.rounded()
```

Actuellement si vous cliquez sur Hit Me directement après avoir lancé l'app, la valeur affichée sera 0, corrigeons ce bug:

- Ajouter un `IBOutlet`:

```swift
@IBOutlet weak var slider: UISlider!
```

- Si vous lancez, l'app va crasher directement car ici le `!` signifie: "promis, il y a un `UISlider` associé", mais ce n'est pas le cas
- Faites un `Clic droit` sur le slider puis sur "New Referencing Outlet", puis glissez comme précédemment vers le ViewController
- mettez à jour la valeur de currentValue dans `viewDidLoad()`

On veut maintenant avoir une valeur à viser:

- Ajouter une propriété `targetValue` initialisée à 0
- Dans `viewDidLoad()`, donnez lui un evaluer aélatoire:

```swift
targetValue = Int.random(in: 1...100)
```

- Affichez ça dans une 2e ligne dans l'alert pour vérifier que ça marche

Actuellement la valeur cible ne change jamais:

- Créér une méthode `startNewRound` qui va:
  - donner une valuer cible aléatoire
  - reset la valeur actuelle à 50
  - reset la valeur du slider à 50
- Utilisez cette méthode au démarrage et à la fin de `showAlert`

- Afficher cette valeur cible dans le label: utilisez un `IBOutlet` et mettez le texte à jour à chaque round
- Affichez maintenant les points gagnés à chaque manche: `100 - abs(targetValue - roundedSliderValue)`
- Accumulez ces points à chaque round dans une propriété `score`
- Relier cette valeur au label associé
- Idem avec le numéro du `round`

## Polish

Le jeu fonctionne bien maintenant, ajoutons quelques détails:

- Afficher 4 phrases différentes dans  le `title` de l'alert en fonction du score obtenu
- Ajouter un bonus de 100 points si le score est parfait
- Ajouter un bonus de 50 si le score est presque parfait (une différence de 1)
- Éxecutez maintenant le démarrage d'un nouveau round seulement après avoir cliqué sur OK: pour cela il faut passer une `lambda` (aussi appelée `closure`) dans le `handler` de l'`UIALertAction` correspondante:

```swift
let action = UIAlertAction(title: "Awesome", style: .default, handler: {
    action in
    // Démarrer un nouveau round
})
```

- Implémentez l'action du bouton "Start Over" qui reset le score, le round

## Navigation

- Créer un 2e ViewController: `FIle > New > File... > CocoaTouch Class` et l'appeler `AboutViewController`
- Ajoutez le dans le StoryBoard
- Ajoutez dedans une TextView et décochez son attribut "Editable"
- AJouter un bouton "Close" en dessous
- Ajoutez une "Segue" en appuyant sur `control` puis cliquer sur le bouton "info" du 1er écran et glisser jusqu'au 2e: dans la popup qui s'affiche, choisir "Present Modally"
- La flèche qui apparaît représente le passage d'une vue à l'autre: changez son attribut "transition" sur "Flip Horizontal"
- Ajouter une `@IBAction` dans `AboutViewController` qui va fermer le 2e écran:

```swift
dismiss(animated: true, completion: nil)
```

- Si vous essayez de relier le bouton, vous allez voir que le lien n'a pas été fait entre le 2e ViewController dans le `StoryBoard` et la *classe* `AboutViewController`: pour cela cliquez sur l'icone jaune et donnez la bonne valeur à l'attribut "Class"

## Style

- Ajoutez les images (sur ce repo) au projet dans `Assets.xcassets`
- Ajoutez une image view dans le Storyboard qui prends toute la vue pour le background: déplacez la en haut de la liste de views pour qu'elle passe derrière les autres
- Stylez les Labels en en sélectionnant plusieurs à la fois:
  - Changez les textes en blanc
  - Ajoutez une ombre noire avec une height de 1
  - Changez la font: "Arial Rounded MT Bold"
  - ... (cf image)
- Style le bouton "Hit Me":
  - Changez "System" tout en haut en "Custom"
  - Donnez le background "Button-normal" pour le state "Default" et  "Button-Highlighted" pour le state "Highlighted"
  - Utilisez la même font que précédemment, size: 20
  - Settez une text color en RGB: 96, 30, 0 (opacité 100%)
  - shadow: color: white, opacité 50%, height: 1
- Stylez le bouton "Start Over":
  - Custom
  - Image: StatOverIcon
  - Background: SmallButton
- Stylez le bouton "Info" de même par vous même
- Pour stlyer le slider, on est obligé de le faire dans le code:
  - Dans viewDidLoad, créer une variable contenant l'image de la cible: tapez `let thumbImageNormal = Image` puis tab dans l'autocompletions ("Image Literal) et l'IDE vous proposera les images
  - utilisez cette image:

    ```swift
    slider.setThumbImage(thumbNormalImage, for: .normal)
    ```

    disponibles en cliquant sur une petite icone bleue
  - faire de même avec l'icone plus sombre pour `thumbImageHighlighted` et l'état `.highlighted`
  - Pour la barre, créez une image "rezizable":

    ```swift
    // Même technique ici avec "Image Literal" récupérer l'image verte:
    let trackLeftImage = [...]
    // Pour que la seule la partie intérieure de l'image soit étirée, pas les bords:
    let insets = UIEdgeInsets(top: 0, left: 14, bottom: 0, right: 14)
    let trackImageResizable = trackLeftImage.resizableImage(withCapInsets: insets)
    slider.setMinimumTrackImage(trackLeftImageResizeable, for: .normal)
    ```

  - Faire exactement pareil mais avec `trackRightImage` et `setMaximumTrackImage`
- Inspirez vous de tout ça pour styler le 2e écran: background, bouton, ...
- Ajoutez l'icone

## Auto Layout

Notre app n'est actuellement pas du tout "Responsive": utilisez des contraintes pour respecter toutes les tailles d'écran en commençant par le 2e écran (plus simple)
