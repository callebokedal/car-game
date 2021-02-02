# Car game

Simple car game built in Konva JS

# Game physics
https://brm.io/game-physics-for-beginners/

# Space ship
https://stackoverflow.com/questions/2890520/2d-spaceship-movement-math

# Unity car video
https://www.youtube.com/watch?v=BRoMtgVWWXQ

# physics - best
https://github.com/spacejack/carphysics2d <- Ska funka!

https://github.com/jongallant/CarSimulator
https://www.asawicki.info/Mirror/Car%20Physics%20for%20Games/Car%20Physics%20for%20Games.html

# Car movement
https://gamedev.stackexchange.com/questions/55857/car-movement-in-2d-dimension


Since I researched this in the past, I will post some code I had converted previously that may help you. This is basically the bare minimum of what you can do to achieve this effect. You will most likely want to add damping, and effects such as braking etc. It also works on a system where you only have two tires, one in front and one in back.

The code can be extended to add the two extra tires, but that will be up to you.

The variables that we will use:

Texture2D vehicle;
Texture2D tire;
Vector2 position;
Vector2 frontTire, backTire;
float direction;
float speed;
float angle;
float wheelBase = 32;

In our Update method:

KeyboardState kbstate = Keyboard.GetState();
if (kbstate.IsKeyDown(Keys.Up))
    speed += 10;
else if (kbstate.IsKeyDown(Keys.Down))
    speed -= 100;
if (kbstate.IsKeyDown(Keys.Left))
    angle -= 0.03f;
else if (kbstate.IsKeyDown(Keys.Right))
    angle += 0.03f;

if (speed < 0)
    speed = 0;
if (speed > 300)
    speed = 300;

frontTire = position + wheelBase / 2 * new Vector2((float)Math.Cos(direction), (float)Math.Sin(direction));
backTire = position - wheelBase / 2 * new Vector2((float)Math.Cos(direction), (float)Math.Sin(direction));

backTire += speed * elapsed * new Vector2((float)Math.Cos(direction), (float)Math.Sin(direction));
frontTire += speed * elapsed * new Vector2((float)Math.Cos(direction + angle), (float)Math.Sin(direction + angle));

position = (frontTire + backTire) / 2;
direction = (float)Math.Atan2(frontTire.Y - backTire.Y, frontTire.X - backTire.X);

And then draw:

spriteBatch.Begin();
spriteBatch.Draw(vehicle, position, null, Color.White, direction, new Vector2(vehicle.Width/2,vehicle.Height/2), 1, SpriteEffects.None, 0);
spriteBatch.Draw(tire, frontTire, null, Color.White, direction, new Vector2(tire.Width/2, tire.Height/2), 1, SpriteEffects.None, 0);
spriteBatch.Draw(tire, backTire, null, Color.White, direction, new Vector2(tire.Width/2, tire.Height/2), 1, SpriteEffects.None, 0);
spriteBatch.End();

This will provide you with the similar behavior as shown in your video. Again, you will need to tinker greatly with this code in order to get the exact behavior you are looking for.
