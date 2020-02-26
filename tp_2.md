
# TP 2: Swift UI

## Création

- Créer un projet vide "Single View App" nommée "Bull's Eyes SwiftUI"
- Le lancer une première fois: ▶
- Utilisez (`cmd + click`) pour afficher la popup de modification
- Personaliser le texte, la couleur, passer en gras:
- Ajouter un bouton: "Hit me !"
- Ajouter une action à ce bouton pour `print` un message dans la console
- Ajouter un `state` dans `ContentView`:

```swift
@State var alertIsVisible = false
```

- Changer sa valeur dans l'action du bouton:

```swift
 self.alertIsVisible = true
 ```

- En dessous du `Button(...){...}`, ajouter:

```swift
.alert(isPresented: $alertIsVisible) { () -> Alert in
  return Alert(
    title: ...,
    message:...,
    dismissButton: .default(Text(...))
  )
}
```

- Dans les settings du projet, décocher l'orientation portrait
- Faire pivoter le Simulator
- Dans `ContentView_Preview`, ajouter:

```swift
.previewLayout(width: 869, height: 414) () //<= iphone 10R )
```

- Passer le volet Preview d'XCode en dessus du volet code: `Editor > Layout > Canvas on bottom`
- Ajouter les Views et utiliser l'IDE pour ne pas trop coder manuellement:
utiliser le bouton "+" et drag & drop (dans le canvas ou dans le code), `cmd + click`, "embed in HStack/Button/...", `Spacer()`, `.padding(.bottom, 20)`

## Gameplay

- ajouter une nouvelle variable au State:

```Swift
@State var sliderValue = 50.0
```

- Utilisez la dans le slider:

```swift
Slider(value: self.$sliderValue, in 1..100)
```

- Utiliser l'interpolation pour afficher cette valeur dans l'`Alert`:

```swift
let interpolatedString = "Hello \(name)!"
```

- Rendre cette valeur plus "humaine" avec `.rounded()` et la stocker dans une variable `let roundedValue: Int` avec un cast

- Ajouter une méthode:

```swift
 func pointsForCurrentRound()  -> Int { return 999 }
```

- Afficher sa valeur dans la seconde ligne de l'alert

- Ajouter une variable d'état qui sera le nombre à cibler, généré aléatoirement:

```swift
@state var target = Int.random(1..100)
```

- Définir une méthode `scoreThisRound()` qui soustrait à 100 la valeur absolue de la différence entre la valeur touchée et la valeur cible
- Créér une méthode  `sliderValueRounded()` pour ne pas dupliquer de code
- Afficher le score total en bas

```swift
@State var score = ...
```

- Changer la target random a chaque tour (dans le dismiss de l'alert)
- Afficher le numéro du round
- Afficher 3 différents textes dans l'alerte en fonction du score
- Ajouter un bonus de 100pts en cas de *perfect* et 50pts en cas de *perfect-1*
- Ajouter la fonctionalité "Start Over"

## Style et Images

- Ajouter les images dans "assets"
- ajouter un background: `.background(Image("Background"), alignement: .center)`
- Ajouter ombre au text, changer la police du texte principal en `Arial Rounded MT Bold` (iosfontd.com) avec `.font(Font.custom(...))`
- Faire pareil sur les autres textes, sans copier coller le meme code partout: pour cela il faut créer un objet `LabelStyle`

```swift
// Définition
struct LabelStyle: ViewModifier {
  func body(content: Content) -> some View {
      // utiliser les modifications de style
  }
}

// Utilisation
.modifier(LabelStyle())
```

- Faire de même pour tous les labels de chiffres: `ValueStyle`
- Pour ne pas dupliquer la partie `.shadow(color: Color.black, radius: 5, x: 2, y: 2)`, faire utiliser aux deux styles un style `Shadow`
- Styliser les boutons:
  - `LargeTextStyle` avec `size: 18` pour Hit Me
  - `SmallTextStyle` avec `size: 12` pour les autres
  - Background: `.background(Image("Button")`
  - Shadow
  - Texte noir
  - Icones:
    - ajouter une `HStack` a l'interieur des boutons
    - `.accentColor` pour la couleur des icones

## Navigation

- Ajouter une nouvelle classe `AboutView`
- Copier la partie `_preview` précédente
- Ajouter des `Text` pour donner des infos sur vous et sur le jeu
- Dans `ScreenDelegate`, changer:

```swift
// Avant
contentView = ContentView()

// Après
contentView = NavigationView {
    ContentView()
  }
  .navigationViewStyle(StackNavigationViewStyle)
```

- Ajouter des titres aux vues:
  - dans ContentView: `.navigationEyesTitle("BullsEyes")`
  - dans AboutView: `.navigationEyesTitle("About")`
- Changer le `Button()` info par un `NavigationLink(destination: AboutView())` avec le meme contenu
- Styliser `AboutView`
- Mettre toute la VStack dans un Group avec le Background
- Ajouter une icone à l'application
