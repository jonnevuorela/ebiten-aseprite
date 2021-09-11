# ebiten-aseprite
Parse and render aseprite animations in ebiten

## Usage
- create an animation in aseprite
- export both the png and the json file
```go
import aseprite "github.com/tducasse/ebiten-aseprite"

//go:embed assets/*
var assetsFolder embed.FS

type Player struct {
	X              float64
	Y              float64
	Sprite         *aseprite.Animation
}

p := &Player{}

p.Sprite = aseprite.Load(
  // name of your aseprite .json file
  "player.json",
  &aseprite.Options{
    // embed your assets folder first
    EmbedFolder: assetsFolder,
    // path to where you store your images
    Root:        "assets/images",
  },
  // which tag to start with
  "idle",
  // we need to pass the whole entity as well
  p,
)

// define a callback function, called every time we reach the end of the current tag
func onAnimLoop(player interface{}, anim *aseprite.Animation) {
  // do something here
}

// attach it to the animation
p.Sprite.OnLoop(onAnimLoop)

// update the animation in your Update call
p.Sprite.Update()
// move to another tag (if it's already playing, it's just ignored)
p.Sprite.SetTag("walk")

// draw the animation in your Draw call
p.Sprite.Draw(p.X, p.Y, screen)
```
