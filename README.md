- 👋 Hi, I’m @genta20080109
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

<!---
genta20080109/genta20080109 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
import curses
import random

curses.initscr()

win = curses.newwin(24,70,0,0)
win.border(0)
curses.noecho()
curses.curs_set(0)
win.keypad(1)
win.nodelay(1)
win.timeout(100)

score = 0

snake = [[12,13],[12,14],[12,15]]

food = [20,20]
win.addch(food[0],food[1],'$')

key = curses.KEY_LEFT

win.addstr(0, 30, 'Snake Game!!')

while True:
    win.addstr(0, 3, '点数: ' + str(score) + ' ')                                  
    win.timeout(100)

    newKey = win.getch()

    if newKey not in [curses.KEY_LEFT,curses.KEY_RIGHT,curses.KEY_UP,curses.KEY_DOWN]:
        key = key

    else:
        key = newKey

    if snake[0][0] == 0 or snake[0][0] == 23 or snake[0][1] == 0 or snake[0][1] == 69:
        break

    if snake[0] in snake[1:]:
        break

    newHead = [snake[0][0],snake[0][1]]

    if key == curses.KEY_DOWN:
        newHead[0] += 1
    if key == curses.KEY_UP:
        newHead[0] -= 1
    if key == curses.KEY_LEFT:
        newHead[1] -= 1
    if key == curses.KEY_RIGHT:
        newHead[1] += 1

    snake.insert(0,newHead)

    if snake[0] == food:
        score += 1
        food = []
        food = [random.randint(1,22),random.randint(1,68)]
        win.addch(food[0],food[1],'$')

    else:
        tail = snake.pop()
        win.addch(tail[0],tail[1],' ')
    win.addch(snake[0][0],snake[0][1],curses.ACS_CKBOARD)

print ('Score: '+str(score))
curses.endwin()
import curses
import random

curses.initscr()

win = curses.newwin(24,70,0,0)
win.border(0)
curses.noecho()
curses.curs_set(0)
win.keypad(1)
win.nodelay(1)
win.timeout(100)

score = 0

snake = [[12,13],[12,14],[12,15]]

food = [20,20]
win.addch(food[0],food[1],'$')

key = curses.KEY_LEFT

win.addstr(0, 30, 'Snake Game!!')

while True:
    win.addstr(0, 3, '点数: ' + str(score) + ' ')                                  
    win.timeout(100)

    newKey = win.getch()

    if newKey not in [curses.KEY_LEFT,curses.KEY_RIGHT,curses.KEY_UP,curses.KEY_DOWN]:
        key = key

    else:
        key = newKey

    if snake[0][0] == 0 or snake[0][0] == 23 or snake[0][1] == 0 or snake[0][1] == 69:
        break

    if snake[0] in snake[1:]:
        break

    newHead = [snake[0][0],snake[0][1]]
from turtle import Turtle, Screen
import random
import time

SIZE = 20

class Square:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    def drawself(self, turtle):

        turtle.goto(self.x - SIZE // 2 - 1, self.y - SIZE // 2 - 1)

        turtle.begin_fill()
        for _ in range(4):
            turtle.forward(SIZE - SIZE // 10)
            turtle.left(90)
        turtle.end_fill()

class Food:
    def __init__(self, x, y):
        self.x = x
        self.y = y
        self.is_blinking = True

    def changelocation(self):
        self.x = random.randint(0, SIZE) * SIZE - 200
        self.y = random.randint(0, SIZE) * SIZE - 200

    def drawself(self, turtle):
        if self.is_blinking:
            turtle.goto(self.x - SIZE // 2 - 1, self.y - SIZE // 2 - 1)
            turtle.begin_fill()
            for _ in range(4):
                turtle.forward(SIZE - SIZE // 10)
                turtle.left(90)
            turtle.end_fill()

    def changestate(self):
        self.is_blinking = not self.is_blinking

class Snake:
    def __init__(self):
        self.headposition = [SIZE, 0]  
        self.body = [Square(-SIZE, 0), Square(0, 0), Square(SIZE, 0)]  
        self.nextX = 1
        self.nextY = 0
        self.crashed = False
        self.nextposition = [self.headposition[0] + SIZE * self.nextX, self.headposition[1] + SIZE * self.nextY]

    def moveOneStep(self):
        if Square(self.nextposition[0], self.nextposition[1]) not in self.body:
            self.body.append(Square(self.nextposition[0], self.nextposition[1]))
            del self.body[0]
            self.headposition[0], self.headposition[1] = self.body[-1].x, self.body[-1].y
            self.nextposition = [self.headposition[0] + SIZE * self.nextX, self.headposition[1] + SIZE * self.nextY]
        else:
            self.crashed = True

    def moveup(self):
        self.nextX, self.nextY = 0, 1

    def moveleft(self):
        self.nextX, self.nextY = -1, 0

    def moveright(self):
        self.nextX, self.nextY = 1, 0

    def movedown(self):
        self.nextX, self.nextY = 0, -1

    def eatFood(self):
        self.body.append(Square(self.nextposition[0], self.nextposition[1]))
        self.headposition[0], self.headposition[1] = self.body[-1].x, self.body[-1].y
        self.nextposition = [self.headposition[0] + SIZE * self.nextX, self.headposition[1] + SIZE * self.nextY]

    def drawself(self, turtle):
        for segment in self.body:
            segment.drawself(turtle)

class Game:
    def __init__(self):
        self.screen = Screen()
        self.artist = Turtle(visible=False)
        self.artist.up()
        self.artist.speed("slowest")

        self.snake = Snake()
        self.food = Food(100, 0)
        self.counter = 0
        self.commandpending = False

        self.screen.tracer(0)

        self.screen.listen()
        self.screen.onkey(self.snakedown, "Down")
        self.screen.onkey(self.snakeup, "Up")
        self.screen.onkey(self.snakeleft, "Left")
        self.screen.onkey(self.snakeright, "Right")

    def nextFrame(self):
        self.artist.clear()

        if (self.snake.nextposition[0], self.snake.nextposition[1]) == (self.food.x, self.food.y):
            self.snake.eatFood()
            self.food.changelocation()
        else:
            self.snake.moveOneStep()

        if self.counter == 10:
            self.food.changestate()
            self.counter = 0
        else:
            self.counter += 1

        self.food.drawself(self.artist)
        self.snake.drawself(self.artist)
        self.screen.update()
        self.screen.ontimer(lambda: self.nextFrame(), 100)

    def snakeup(self):
        if not self.commandpending:
            self.commandpending = True
            self.snake.moveup()
            self.commandpending = False

    def snakedown(self):
        if not self.commandpending:
            self.commandpending = True
            self.snake.movedown()
            self.commandpending = False

    def snakeleft(self):
        if not self.commandpending:
            self.commandpending = True
            self.snake.moveleft()
            self.commandpending = False

    def snakeright(self):
        if not self.commandpending:
            self.commandpending = True
            self.snake.moveright()
            self.commandpending = False

game = Game()

screen = Screen()

screen.ontimer(lambda: game.nextFrame(), 100)

screen.mainloop()

    if key == curses.KEY_DOWN:
        newHead[0] += 1
    if key == curses.KEY_UP:
        newHead[0] -= 1
    if key == curses.KEY_LEFT:
        newHead[1] -= 1
    if key == curses.KEY_RIGHT:
        newHead[1] += 1

    snake.insert(0,newHead)

    if snake[0] == food:
        score += 1
        food = []
        food = [random.randint(1,22),random.randint(1,68)]
        win.addch(food[0],food[1],'$')

    else:
        tail = snake.pop()
        win.addch(tail[0],tail[1],' ')
    win.addch(snake[0][0],snake[0][1],curses.ACS_CKBOARD)

print ('Score: '+str(score))
curses.endwin()
from pygame.locals import *
from random import randint
import pygame
import time

class Apple:
    x = 0
    y = 0
    step = 44

    def __init__(self,x,y):
        self.x = x * self.step
        self.y = y * self.step

    def draw(self, surface, image):
        surface.blit(image,(self.x, self.y)) 

class Player:
    x = [0]
    y = [0]
    step = 44
    direction = 0
    length = 3

    updateCountMax = 2
    updateCount = 0

    def __init__(self, length):
       self.length = length
       for i in range(0,2000):
           self.x.append(-100)
           self.y.append(-100)

       self.x[1] = 1*44
       self.x[2] = 2*44

    def update(self):

        self.updateCount = self.updateCount + 1
        if self.updateCount > self.updateCountMax:

            for i in range(self.length-1,0,-1):
                self.x[i] = self.x[i-1]
                self.y[i] = self.y[i-1]

            if self.direction == 0:
                self.x[0] = self.x[0] + self.step
            if self.direction == 1:
                self.x[0] = self.x[0] - self.step
            if self.direction == 2:
                self.y[0] = self.y[0] - self.step
            if self.direction == 3:
                self.y[0] = self.y[0] + self.step

            self.updateCount = 0


    def moveRight(self):
        self.direction = 0

    def moveLeft(self):
        self.direction = 1

    def moveUp(self):
        self.direction = 2

    def moveDown(self):
        self.direction = 3 

    def draw(self, surface, image):
        for i in range(0,self.length):
            surface.blit(image,(self.x[i],self.y[i])) 

class Game:
    def isCollision(self,x1,y1,x2,y2,bsize):
        if x1 >= x2 and x1 <= x2 + bsize:
            if y1 >= y2 and y1 <= y2 + bsize:
                return True
        return False

class App:

    windowWidth = 800
    windowHeight = 600
    player = 0
    apple = 0

    def __init__(self):
        self._running = True
        self._display_surf = None
        self._image_surf = None
        self._apple_surf = None
        self.game = Game()
        self.player = Player(3) 
        self.apple = Apple(5,5)

    def on_init(self):
        pygame.init()
        self._display_surf = pygame.display.set_mode((self.windowWidth,self.windowHeight), pygame.HWSURFACE)

        pygame.display.set_caption('Pygame pythonspot.com example')
        self._running = True
        self._image_surf = pygame.image.load("block.jpg").convert()
        self._apple_surf = pygame.image.load("block.jpg").convert()

    def on_event(self, event):
        if event.type == QUIT:
            self._running = False

    def on_loop(self):
        self.player.update()

        for i in range(0,self.player.length):
            if self.game.isCollision(self.apple.x,self.apple.y,self.player.x[i], self.player.y[i],44):
                self.apple.x = randint(2,9) * 44
                self.apple.y = randint(2,9) * 44
                self.player.length = self.player.length + 1


        for i in range(2,self.player.length):
            if self.game.isCollision(self.player.x[0],self.player.y[0],self.player.x[i], self.player.y[i],40):
                print("You lose! Collision: ")
                print("x[0] (" + str(self.player.x[0]) + "," + str(self.player.y[0]) + ")")
                print("x[" + str(i) + "] (" + str(self.player.x[i]) + "," + str(self.player.y[i]) + ")")
                exit(0)

        pass

    def on_render(self):
        self._display_surf.fill((0,0,0))
        self.player.draw(self._display_surf, self._image_surf)
        self.apple.draw(self._display_surf, self._apple_surf)
        pygame.display.flip()

    def on_cleanup(self):
        pygame.quit()

    def on_execute(self):
        if self.on_init() == False:
            self._running = False

        while( self._running ):
            pygame.event.pump()
            keys = pygame.key.get_pressed() 

            if (keys[K_RIGHT]):
                self.player.moveRight()

            if (keys[K_LEFT]):
                self.player.moveLeft()

            if (keys[K_UP]):
                self.player.moveUp()

            if (keys[K_DOWN]):
                self.player.moveDown()

            if (keys[K_ESCAPE]):
                self._running = False

            self.on_loop()
            self.on_render()

            time.sleep (50.0 / 1000.0);
        self.on_cleanup()

if __name__ == "__main__" :
    theApp = App()
    theApp.on_execute()
    
