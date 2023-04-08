Hello World
====

A lovly game where you can have some fun with UFOs 🛸 


Features 
-----
  - lovly bullet to kill 🛸 
  - beautiful fire and💥 
  - cute looking tank with ☄️
  - friendly 👾 that kills you


Method 
----
 - click space to shoot
 - click left ➡ to move 👈 
 - click right ➡ to move 👉 


Resources 
---- 
 - Kenny.com
 - python
 - pycat 🐱🐈🙀 

Code
----
from pycat.core import Window, Sprite, Scheduler, RotationMode, Label, Color, KeyCode, Player
import random
from random import randint
import random


window = Window(background_image='bg.jpg')
window.background_sprite.scale = 1.6



def get_random_file_from_directory(path):
    from os import listdir
    from os.path import realpath, dirname, join, isfile
    script_path = realpath(dirname(__file__))
    abs_img_dir = join(script_path,path)
    files = [f for f in listdir(abs_img_dir) if isfile(join(abs_img_dir, f))]
    return join(path, random.choice(files))	



class UFO(Sprite):
    def on_create(self):
        self.x = 1300
        self.y = 550
        self.scale = 0.7
        self.image = get_random_file_from_directory('ufos/')
        self.add_tag('ufo')
    def on_update(self, dt):
        self.x += -4
        if self.x < 0:
            self.delete()



class EUFO(Sprite):
    def on_create(self):
        self.x = 1300
        self.y = random.randint(50, 700)
        self.scale = 0.7
        self.image = get_random_file_from_directory('ufos/')
        self.add_tag('ufo')
    def on_update(self, dt):
        self.x += -6
        self.y += -2
        if self.y < 0:
            self.delete()


class Tank(Sprite):
    def on_create(self):
        self.x = 20
        self.y = 35
        self.scale = 1
        self.image = get_random_file_from_directory('tanks/')
    def on_update(self, dt):
        if window.is_key_pressed(KeyCode.LEFT):
            self.x += -3
        if window.is_key_pressed(KeyCode.RIGHT):
            self.x += 3
        if window.is_key_down(KeyCode.SPACE):
            window.create_sprite(Shoot, position = self.position)
        for sprite in self.get_touching_sprites_with_tag('ufo'):
            sprite.delete()
            self.scale += 1
            self.y += 30
            window.create_sprite(Fire, position = self.position) 


class Fire(Sprite):
    def on_create(self):
        self.image = 'fire.png'
        self.timer = 0 
        self.scale = 0.7
        self.layer = 10000
    def on_update(self, dt):
        self.timer += dt
        if self.timer > 1 :
            self.delete() 
        


class Shoot(Sprite):
    def on_create(self):
        self.x = 10
        self.color = Color.ROSE    
        self.scale = 10
        self.layer = -10 
    def on_update(self, dt):
        self.y += 7
        if self.is_touching_window_edge():
            self.delete()
        for sprite in self.get_touching_sprites_with_tag('ufo'):
            sprite.delete()
            self.delete()



Scheduler.update(lambda: window.create_sprite(UFO),0.6)
Scheduler.update(lambda: window.create_sprite(EUFO),3)


tank = window.create_sprite(Tank)
ufo = window.create_sprite(UFO)
eufo = window.create_sprite(EUFO)
fire = window.create_sprite(Fire)


window.run()
