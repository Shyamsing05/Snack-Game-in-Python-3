# Sneck-Game-in-Python-3
#I have developed snack game using python3

import turtle
import random
import time

delay = 000.1
score = 0
high_score = 0
bodies = []

# Setup screen
s1 = turtle.Screen()
s1.title("Snake Game")
s1.bgcolor("light blue")
s1.setup(width=600, height=600)
s1.tracer(0)  # turn off automatic updates

# Head
h1 = turtle.Turtle()
h1.shape("circle")
h1.color("red")
h1.penup()
h1.goto(0, 0)
h1.direction = "stop"

# Food
f1 = turtle.Turtle()
f1.shape("square")
f1.color("blue")
f1.penup()
f1.goto(200, 200)

# Scoreboard
s2 = turtle.Turtle()
s2.hideturtle()
s2.penup()
s2.goto(-250, 250)
s2.write(f"Score: {score}  |  Highest Score: {high_score}", align="left", font=("Arial", 16, "normal"))

# Movement functions
def move_up():
    if h1.direction != "down":
        h1.direction = "up"

def move_down():
    if h1.direction != "up":
        h1.direction = "down"

def move_left():
    if h1.direction != "right":
        h1.direction = "left"

def move_right():
    if h1.direction != "left":
        h1.direction = "right"

def move():
    if h1.direction == "up":
        y = h1.ycor()
        h1.sety(y + 20)
    if h1.direction == "down":
        y = h1.ycor()
        h1.sety(y - 20)
    if h1.direction == "left":
        x = h1.xcor()
        h1.setx(x - 20)
    if h1.direction == "right":
        x = h1.xcor()
        h1.setx(x + 20)

# Key bindings
s1.listen()
s1.onkey(move_up, "Up")
s1.onkey(move_down, "Down")
s1.onkey(move_left, "Left")
s1.onkey(move_right, "Right")
# optional: stop
# s1.onkey(lambda: setattr(h1, 'direction', 'stop'), "space")

# Main game loop
while True:
    s1.update()

    # Border collision / wrapping
    if h1.xcor() > 290:
        h1.setx(-290)
    if h1.xcor() < -290:
        h1.setx(290)
    if h1.ycor() > 290:
        h1.sety(-290)
    if h1.ycor() < -290:
        h1.sety(290)

    # Check food collision
    if h1.distance(f1) < 20:
        # Move food to new random spot
        x = random.randint(-280, 280)
        y = random.randint(-280, 280)
        f1.goto(x, y)

        # Add a new body segment
        b = turtle.Turtle()
        b.speed(0)
        b.shape("square")
        b.color("yellow")
        b.penup()
        bodies.append(b)

        # Increase score
        score += 10
        if score > high_score:
            high_score = score

        # Update scoreboard
        s2.clear()
        s2.write(f"Score: {score}  |  Highest Score: {high_score}", align="left", font=("Arial", 16, "normal"))

        # Increase speed a bit (reduce delay)
        delay = max(0.02, delay - 0.005)

    # Move the body segments in reverse order
    for index in range(len(bodies)-1, 0, -1):
        x = bodies[index-1].xcor()
        y = bodies[index-1].ycor()
        bodies[index].goto(x, y)

    # Move first body to where head is
    if bodies:
        x = h1.xcor()
        y = h1.ycor()
        bodies[0].goto(x, y)

    move()

    # Check collision of head with body
    for b in bodies:
        if b.distance(h1) < 20:
            time.sleep(1)
            h1.goto(0, 0)
            h1.direction = "stop"

            # Hide bodies
            for body in bodies:
                body.hideturtle()
            bodies.clear()

            score = 0
            s2.clear()
            s2.write(f"Score: {score}  |  Highest Score: {high_score}", align="left", font=("Arial", 16, "normal"))

            # Reset delay
            delay = 000.1

    time.sleep(delay)
    
