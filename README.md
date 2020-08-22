# Space_Invader_Or-_Die
Mission: to destroy the 30 aliens instructions: 'left' arrow to go left               'right'arrow to go right               'up' arrow to go up'down' arrow to go down               'space bar ' to shoot if you collide with an alien you die! DONT FAIL US!!!

import turtle
import winsound
import math
import random

#Set Screen
sc=turtle.Screen()
sc.bgcolor("black")
sc.title("Space Invaders")
sc.bgpic("space_invaders_background.gif")
sc.tracer(0)

#Register shapes
turtle.register_shape("invader.gif")
turtle.register_shape("player.gif")

#Draw Border
border_pen=turtle.Turtle()
border_pen.speed(0)
border_pen.color("white")
border_pen.penup()
border_pen.setposition(-300,-300)
border_pen.pendown()
border_pen.pensize(3)

for side in range(4):
 border_pen.fd(600)
 border_pen.lt(90)

border_pen.hideturtle()

#Score
score=0

score_pen=turtle.Turtle()
score_pen.speed(0)
score_pen.color("white")
score_pen.penup()
score_pen.setposition(-290,280)
scorestring="Score : %s" %score
score_pen.write(scorestring,False,align="left",font=("Torque",14,"normal"))
score_pen.hideturtle()

#Create player
player=turtle.Turtle()
player.color("blue")
player.shape("player.gif")
player.speed(0)
player.penup()
player.setposition(0,-250)
player.setheading(90)

playerspeed=20

#Create Aliens
number_of_enemies=30
enemies=[]

for i in range(number_of_enemies):
 enemies.append(turtle.Turtle())

 enemy_start_x= -225
 enemy_start_y= 250
 enemy_number=  0

for enemy in enemies:
 enemy.color("red")
 enemy.shape("invader.gif")
 enemy.penup()
 enemy.speed(0)
 x=enemy_start_x + (50 * enemy_number)
 y=enemy_start_y
 enemy.setposition(x,y)

 #Update enemy number
 enemy_number += 1

 if enemy_number == 10 :
     enemy_start_y -= 50
     enemy_number = 0
 
 enemyspeed=0.1

#Create players weapon
beam=turtle.Turtle()
beam.color("yellow")
beam.shape("triangle")
beam.penup()
beam.speed(0)
beam.setheading(90)
beam.shapesize(0.5,0.5)
beam.hideturtle()

beamspeed=7

#Define beam state
#armed-ready to engage
#engage-beam is firing
beamstate="armed"

#Move player  in 'x' and 'y' axes
def move_left():
 x=player.xcor()
 x-=playerspeed
 if x<-280:
  x=-280
 player.setx(x)

def move_right():
 x=player.xcor()
 x += playerspeed
 if x>280:
  x=280
 player.setx(x)

def move_up():
 y=player.ycor()
 y+=playerspeed
 if y>280:
  y=280
 player.sety(y)

def move_down():
 y=player.ycor()
 y-=playerspeed
 if y<-280:
  y=-280
 player.sety(y)


def engage_beam():
 global beamstate
 if beamstate=="armed":
  winsound.PlaySound("laser.wav",winsound.SND_ASYNC)
  beamstate="engage"
  x=player.xcor()
  y=player.ycor() + 10
  beam.setposition(x,y)
  beam.showturtle()

def isCollision(t1,t2):
 distance=math.sqrt(math.pow(t1.xcor()-t2.xcor(),2)+math.pow(t1.ycor()-t2.ycor(),2))
 if distance < 15:
  return True
 else:
  return False 

#Keyboard bind it
turtle.listen()
turtle.onkey(move_left,"Left")
turtle.onkey(move_right,"Right")
turtle.onkey(move_up,"Up")
turtle.onkey(move_down,"Down")
turtle.onkey(engage_beam,"space")

#Main game loop
while True:

 sc.update()
#Move the Alien
 for enemy in enemies: 
      x=enemy.xcor()
      x +=enemyspeed
      enemy.setx(x)

    #Move enemy back and down
      if enemy.xcor()>280 :
        for e in enemies:
            y=e.ycor()
            y -=40
            e.sety (y)
        enemyspeed *= -1

     
      if enemy.xcor()<-280 :
        for e in enemies:   
            y=e.ycor()
            y -=40
            e.sety (y)
        enemyspeed *= -1


 #Border checking
      if beam.ycor() > 275:
       beam.hideturtle()
       beamstate="armed"

      if isCollision(beam,enemy):
       beam.hideturtle()
       winsound.PlaySound("explosion.wav",winsound.SND_ASYNC)
       beamstate="armed"
       beam.setposition(0,-400)
       enemy.setposition(0,100000)
       score += 10
       scorestring="Score : %s" %score
       score_pen.clear()
       score_pen.write(scorestring,False,align="left",font=("Torque",14,"normal"))
       


      if isCollision(player,enemy):
       winsound.PlaySound("explosion.wav",winsound.SND_ASYNC)
       player.hideturtle()
       enemy.hideturtle()
       enemy.setposition(0,100000)
       sc.bgpic("game over.gif")
       break
 
 #Engage beam
 y=beam.ycor()
 y +=beamspeed
 beam.sety(y)

 #Border checking
 if beam.ycor() > 275:
  beam.hideturtle()
  beamstate="armed"

 if score==300 :
  sc.bgpic("game win.gif")
  player.hideturtle()

 



delay=raw_input("Press Enter to finish")
