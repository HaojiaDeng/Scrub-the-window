Window Dust Wipe - Prototype

Files added:
- Assets/Scripts/DustController.cs : creates and manages a runtime Texture2D as the dust overlay, allows erasing with a circular brush.
- Assets/Scripts/Brush.cs : handles mouse input, measures brush speed (lose if too fast), paints the dust, checks fail-points.
- Assets/Scripts/GameManager.cs : simple singleton to manage win/lose state and parameters.
- Assets/Scripts/WinPoint.cs : attach to empty GameObjects to mark 'danger points' on the glass. Touching them loses the game.

Quick setup (in Unity Editor):
1. Create a new Scene (Main) or open an existing one.
2. Create an empty GameObject named `GameManager` and attach `GameManager` component.
3. Create an empty GameObject named `DustLayer` at (0,0,0). Add a SpriteRenderer to it.
   - Remove any sprite asset; the `DustController` will create a runtime sprite.
   - Add the `DustController` component to `DustLayer`.
   - Tweak `textureWidth`, `textureHeight`, `pixelsPerUnit`, and `dustColor` if needed.
4. Create another empty GameObject named `BrushController` and attach the `Brush` script to it.
   - Assign `DustLayer` (the `DustController` component) to the `dustController` field.
   - Set `brushRadius` and `maxAllowedSpeed` to tune gameplay.
5. Place some fail points:
   - Create empty GameObjects where you want dangerous spots and attach `WinPoint` to each.
   - Adjust the `radius` to set how close is considered a hit.
6. Camera setup:
   - Use an orthographic camera that frames the `DustLayer` area. The `Brush` script uses `Camera.main` for screen-to-world.
7. UI (optional):
   - Add a Canvas and simple Text for win/lose. Hook up `GameManager.Win()` and `GameManager.Lose()` to show UI.

How it works (overview):
- The `DustController` creates a texture and assigns it to the GameObject's SpriteRenderer.
- `Brush` converts mouse position to world position, calculates stroke speed, and erases the dust texture along the stroke by drawing transparent circles into the texture.
- If brush speed > `maxAllowedSpeed` the `GameManager` triggers a loss (glass shatters).
- If the brush touches a `WinPoint` (within `WinPoint.radius`) the `GameManager` triggers a loss.
- If cleared percent >= `GameManager.winClearPercent` the player wins.

Notes & next steps:
- This is a small prototype; performance is fine for prototyping sizes (e.g. 512..2048). For production you may switch to shaders or RenderTextures.
- You can add a simple glass-break visual by swapping the dust sprite and playing an animation in `GameManager.Lose()`.

Enjoy! Open the scene and play with the parameters to tune difficulty.