import pygame,random
import pygame._view
pygame.init()




screen = pygame.display.set_mode((640,480))

class Splash(pygame.sprite.Sprite):
	def __init__(self):
	    pygame.sprite.Sprite.__init__(self)
	    self.image = pygame.image.load("sprites/splash.png")
            self.image = self.image.convert()
            self.rect = self.image.get_rect()
            self.rect.center = (320,240)

        def update(self):
            self.rect.centerx += 0
            self.rect.centery += 0



class Ocean(pygame.sprite.Sprite):
      def __init__(self):
          pygame.sprite.Sprite.__init__(self)
          self.image = pygame.image.load("sprites/ocean.png")
          self.image = self.image.convert()
          self.rect = self.image.get_rect()
          self.rect.center = (320,240)
          self.dy = 3

      def update(self):
          self.rect.centery += self.dy
          if self.rect.top >= 0 :
             self.rect.bottom = screen.get_height()

class Ship(pygame.sprite.Sprite):
      def __init__(self):
          pygame.sprite.Sprite.__init__(self)
          self.image = pygame.image.load("sprites/ship.png")
          self.image.set_colorkey((0,0,0))
          self.rect = self.image.get_rect()
          self.rect.centery = 400

      def update(self):
          (mx,my) = pygame.mouse.get_pos()
          self.rect.centerx = mx


class Enemy(pygame.sprite.Sprite):
      def __init__(self):
          pygame.sprite.Sprite.__init__(self)
          self.image = pygame.image.load("sprites/enemy.png")
          self.rect = self.image.get_rect()
          self.dy = 5
          self.reset()
          self.rect.centerx = random.randint(0,screen.get_width())
          self.rect.centery = 0
            
      def update(self):
          self.rect.centery += self.dy
          if self.rect.top >= screen.get_height():
             self.reset()

      def reset(self):
          self.dy = 7
          self.rect.centerx = random.randint(0,screen.get_width())
          self.rect.centery = 0   

enemy = Enemy()

class Rocket(pygame.sprite.Sprite):
      def __init__(self):
          pygame.sprite.Sprite.__init__(self)
          self.image = pygame.image.load("sprites/rocket.png")
          self.rect = self.image.get_rect()  
          self.dy = 0
          self.mx = 0
          self.reset()         
     
      def reset(self):
          self.rect.centerx = self.mx
          self.rect.centery = 400   

class ERocket(pygame.sprite.Sprite):
      def __init__(self):
          pygame.sprite.Sprite.__init__(self)
          self.image = pygame.image.load("sprites/enemyrocket.png")
          self.image.set_colorkey((255,255,255))
          self.rect = self.image.get_rect()
          self.dy = 0
          self.reset(enemy)

      def update(self,enemy):
          self.rect.centerx = enemy.rect.centerx 
          self.rect.centery = enemy.rect.centery + self.dy 

      def reset(self,enemy):
          self.rect.center = enemy.rect.center   


class Plane(pygame.sprite.Sprite):
      def __init__(self):
          pygame.sprite.Sprite.__init__(self)
          self.image = pygame.image.load("sprites/plane2.png")
          self.rect = self.image.get_rect()
          self.rect.centery = 0
          self.rect.centerx = random.randint(0,screen.get_width()) 
          self.dy = 7
          self.reset()
      
      def update(self):
          self.rect.centery += self.dy
          if self.rect.bottom >= screen.get_height():
             self.reset()

      def reset(self):
          self.rect.centery = 0
          self.rect.centerx = random.randint(0,screen.get_width())

plane = Plane()
 
class Plane_rocket1(pygame.sprite.Sprite):
      def __init__(self):
          pygame.sprite.Sprite.__init__(self)
          self.image = pygame.image.load("sprites/enemyrocket.png")
          self.rect = self.image.get_rect()
          self.dy = 0
          self.reset(plane)

      def update(self,plane):
          self.rect.centerx = plane.rect.centerx + 20
          self.rect.centery = plane.rect.centery + self.dy

      def reset(self,plane):
          self.rect.center = plane.rect.center

class Plane_rocket2(pygame.sprite.Sprite):
      def __init__(self):
          pygame.sprite.Sprite.__init__(self)
          self.image = pygame.image.load("sprites/enemyrocket.png")
          self.rect = self.image.get_rect()
          self.dy = 0
          self.reset(plane)

      def update(self,plane):
          self.rect.centerx = plane.rect.centerx - 20
          self.rect.centery = plane.rect.centery + self.dy

      def reset(self,plane):
          self.rect.center = plane.rect.center

class Scoreboard(pygame.sprite.Sprite):
      def __init__(self):
          pygame.sprite.Sprite.__init__(self)
          self.font = pygame.font.SysFont("none",50)
          self.center = (0,0)
          self.life = 20
          self.point = 0 
          self.rocket = 20 
          self.reset()

      def update(self):
          self.text = "Life - %s Rocket - %s Points - %s" % (self.life,self.rocket,self.point)
          self.image = self.font.render(self.text,1,(255,0,0))
          self.rect = self.image.get_rect()
          self.rect.center = self.center

      def reset(self):
          self.life = 20
          self.rocket = 20
          self.point = 0

class Cargo(pygame.sprite.Sprite):
      def __init__(self):
          pygame.sprite.Sprite.__init__(self)
          self.image = pygame.image.load("sprites/cargo.png")
          self.rect = self.image.get_rect()
          self.rect.centery = -110
          self.rect.centerx = random.randint(0,screen.get_width())
          self.dy = 3
          self.reset()
          
      def update(self):
          self.rect.centery += self.dy
          if self.rect.top >= screen.get_height():
             self.reset()
       
      def reset(self):
          self.rect.centery = -110
          self.rect.centerx = random.randint(0,screen.get_width())     

cargo = Cargo()

class Cargo_rocket1(pygame.sprite.Sprite):
      def __init__(self):
          pygame.sprite.Sprite.__init__(self)
          self.image = pygame.image.load("sprites/enemyrocket.png")
          self.rect = self.image.get_rect()
          self.reset(cargo)   
          self.dy = 0   

      def update(self,cargo):
          self.rect.centerx = cargo.rect.centerx - 16 
          self.rect.centery = cargo.rect.centery - 25 + self.dy
  
      def reset(self,cargo):
          self.rect.centerx = cargo.rect.centerx - 16
          self.rect.centery = cargo.rect.centery - 25    

class Cargo_rocket2(pygame.sprite.Sprite):
      def __init__(self):
          pygame.sprite.Sprite.__init__(self)
          self.image = pygame.image.load("sprites/enemyrocket.png")
          self.rect = self.image.get_rect()
          self.reset(cargo)
          self.dy = 0

      def update(self,cargo):
          self.rect.centerx = cargo.rect.centerx + 16 
          self.rect.centery = cargo.rect.centery - 25 + self.dy

      def reset(self,cargo):
          self.rect.centerx = cargo.rect.centerx + 16
          self.rect.centery = cargo.rect.centery - 25
        

class Cargo_rocket3(pygame.sprite.Sprite):
      def __init__(self):
          pygame.sprite.Sprite.__init__(self)
          self.image = pygame.image.load("sprites/enemyrocket.png")
          self.rect = self.image.get_rect()
          self.reset(cargo)
          self.dy = 0

      def update(self,cargo):
          self.rect.centerx = cargo.rect.centerx
          self.rect.centery = cargo.rect.centery - 55 + self.dy
          
      def reset(self,cargo):
          self.rect.centerx = cargo.rect.centerx
          self.rect.centery = cargo.rect.centery - 55

class Ammo(pygame.sprite.Sprite):
      def __init__(self,cargo):
          pygame.sprite.Sprite.__init__(self)
          self.image = pygame.image.load("sprites/ammo.png")
          self.rect = self.image.get_rect()
          self.count = 2
          self.rect.centerx = cargo.rect.centerx 
          self.rect.centery = cargo.rect.centery - 75
          self.reset(cargo)
 
      def update(self,cargo):
          self.rect.centery += cargo.dy
          if self.rect.top >= screen.get_height():
             self.reset(cargo)

      def reset(self,cargo):
          self.rect.centerx = cargo.rect.centerx 
          self.rect.centery = cargo.rect.centery - 75

           

########################################################################################################

class Label(pygame.sprite.Sprite):
      def __init__(self):
          pygame.sprite.Sprite.__init__(self)
          self.font = pygame.font.SysFont("alpacasolidify",30)
          self.text = ""
          self.center = (200,200)
          
      def update(self):
          self.image = self.font.render(self.text,1,(0,0,0))
          self.rect = self.image.get_rect()
          self.rect.center = self.center

def Top_scores(list,list2):
    screen = pygame.display.set_mode((640,480))
    background = pygame.image.load("sprites/pirate.jpg")
    screen.blit(background,(0,0))

    clock = pygame.time.Clock() 
    truth = True

    label = Label()
    label.font = pygame.font.SysFont("alfaslabone",60)
    label.text = "BEST SHIPS"
    label.center = (310,30)

    label_1 = Label()
    label_1.font = pygame.font.SysFont("bernardcondensed",40)
    label_1.text = "RANK"
    label_1.center = (50,100)

    label_2 = Label()
    label_2.font = pygame.font.SysFont("bernardcondensed",40)
    label_2.text = "NAME"
    label_2.center = (310,100)

    label_3 = Label()
    label_3.font = pygame.font.SysFont("bernardcondensed",40)
    label_3.text = "SCORE"
    label_3.center = (570,100)

    #############################################################
    first = Label()
    first.font = pygame.font.SysFont("bernardcondensed",35)
    first.text = "1st"
    first.center = (50,160)

    f_name = Label()
    f_name.font = pygame.font.SysFont("bernardcondensed",35)
    f_name.text = "%s" % list2[0][0]
    f_name.center = (310,160)

    f_score = Label()
    f_score.font = pygame.font.SysFont("bernardcondensed",35)
    f_score.text = "%s" % list[0]
    f_score.center = (570,160)
    #############################################################

    #############################################################
    second = Label()
    second.font = pygame.font.SysFont("bernardcondensed",35)
    second.text = "2nd"
    second.center = (50,235)


    s_name = Label()
    s_name.font = pygame.font.SysFont("bernardcondensed",35)
    s_name.text = "%s" % list2[1][0]
    s_name.center = (310,235)

    s_score = Label()
    s_score.font = pygame.font.SysFont("bernardcondensed",35)
    s_score.text = "%s" % list[1]
    s_score.center = (570,235)
    #############################################################

    ###############################################################
    third = Label()
    third.font = pygame.font.SysFont("bernardcondensed",35)
    third.text = "3rd"
    third.center = (50,310)

    t_name = Label()
    t_name.font = pygame.font.SysFont("bernardcondensed",35)
    t_name.text = "%s" % list2[2][0]
    t_name.center = (310,310)

    t_score = Label()
    t_score.font = pygame.font.SysFont("bernardcondensed",35)
    t_score.text = "%s" % list[2]
    t_score.center = (570,310)
    ###############################################################

    ###############################################################
    fourth = Label()
    fourth.font = pygame.font.SysFont("bernardcondensed",35)
    fourth.text = "4th"
    fourth.center = (50,385)


    fo_name = Label()
    fo_name.font = pygame.font.SysFont("bernardcondensed",35)
    fo_name.text = "%s" % list2[3][0]
    fo_name.center = (310,385)

    fo_score = Label()
    fo_score.font = pygame.font.SysFont("bernardcondensed",35)
    fo_score.text = "%s" % list[3]
    fo_score.center = (570,385)
    ###############################################################

    ###############################################################
    fifth = Label()
    fifth.font = pygame.font.SysFont("bernardcondensed",35)
    fifth.text = "5th"
    fifth.center = (50,460)


    fi_name = Label()
    fi_name.font = pygame.font.SysFont("bernardcondensed",35)
    fi_name.text = "%s" % list2[4][0]
    fi_name.center = (310,460)

    fi_score = Label()
    fi_score.font = pygame.font.SysFont("bernardcondensed",35)
    fi_score.text = "%s" % list[4]
    fi_score.center = (570,460)



    allsprites = pygame.sprite.LayeredUpdates(label,label_1,label_2,label_3,first,second,third,fourth,fifth,f_name,f_score,s_name,s_score,t_name,t_score,fo_name,fo_score,fi_name,fi_score)

    while truth == True:
          clock.tick(30)

          for event in pygame.event.get():
              if event.type == pygame.QUIT:
                 truth = False
	      elif event.type == pygame.MOUSEBUTTONDOWN:
		   truth = False
              elif event.type == pygame.KEYDOWN:
                   keyname = pygame.key.name(event.key)
                   if keyname == "return":
                      truth = False

          allsprites.clear(screen,background)
          allsprites.update() 
          allsprites.draw(screen)
          pygame.display.flip()

    return True

    

 
def Score(score):
    screen = pygame.display.set_mode((640,480))
    background = pygame.image.load("sprites/pirate.jpg")
    screen.blit(background,(0,0))
    

    clock = pygame.time.Clock()
    truth = True
 
    label_1 = Label()
    x = 290
    label_1.font = pygame.font.SysFont("agencyfb",50)
    label_1.center = (290,310)


    label_2 = Label()
    label_2.center = (290,250)
    label_2.font = pygame.font.SysFont("agencyfb",70)
    label_2.text = "YOUR NAME"

    label_3 = Label()
    label_3.center = (290,80)
    label_3.font = pygame.font.SysFont("agencyfb",70)
    label_3.text = "YOUR SCORE"

    label_4 = Label()
    label_4.center = (290,150)
    label_4.font = pygame.font.SysFont("agencyfb",50)
    label_4.text = "%s" % score
    
    allsprites = pygame.sprite.LayeredUpdates(label_1,label_2,label_3,label_4)
   
    name = ""
    caps = False
    while truth == True:
          clock.tick(30)

          for event in pygame.event.get():
              if event.type == pygame.QUIT:
                 truth = False
              if event.type == pygame.KEYDOWN: 
                 keyname = pygame.key.name(event.key)
                 if keyname == "backspace":
                    name = name[:len(name)-1]
                    label_1.text = name
         
                    label_1.center = (x,320)
                 elif keyname == "space":
                    name = name + " "
                    label_1.text = name
                    
                    label_1.center = (x,320)
                 elif keyname == "caps lock":
                      if caps == False:
                         caps = True
                      elif caps == True:
                           caps = False 
                 elif keyname == "return":
                      truth = False      
                 else:
                     if caps == False:
                        name = name + keyname
                        label_1.text = name
                        
                        label_1.center = (x,320)
                     elif caps == True:
                          name = name + keyname.upper()
                          label_1.text = name
                          
                          label_1.center = (x,320)

                             
          
          allsprites.clear(screen,background)
          allsprites.update()
          allsprites.draw(screen)
          pygame.display.flip()

    print name

    sh = open("name.txt","a")
    sh.write("\n%s-%s" % (name,score))
    sh.close()

    fs = open("name.txt","r")
    list_1 = []
    list_2 = [] 
    list_3 = []
    list_4 = []
    list_5 = []
    list_6 = []
    list_7 = []
    dic = {} 

    for c in fs:
        list_1.append(c)
    for d in range(0,len(list_1)):
        if d != len(list_1)-1:
           list_2.append(list_1[d][:len(list_1[d])-1])
        elif d == len(list_1) - 1:
             list_2.append(list_1[d])
    for e in list_2:
        l = []
        l = e.split("-")
        l[1] = int(l[1])
        list_6.append(l)      
        list_3.append(int(l[1]))
        list_3.sort()     

    list_4 = list_3[::-1]
    for c in range(5):
        for d in list_6:
            if list_4[c] == d[1]:
               list_5.append(list_4[c])
               list_7.append(d)
               list_6.remove(d)
               break               
 
    print list_5,list_7,list_6
    Top_scores(list_5,list_7)
    return True    
      

#############################################################################################
  
score = 0 

def main(score): 
    truth1 = True
    
    ##########################################################################################

    rs = open("score.txt","r")
    list = []
    list2 = []
    list3 = []
    for c in rs:
        list.append(c)
    for d in range(0,len(list)):
        if d != len(list) - 1:
           list2.append(list[d][:len(list[d])-1])
        if d == len(list) - 1:
           list2.append(list[d][:len(list[d])])
    for e in list2:
        list3.append(int(e))
        list3.sort()
    score = list3[len(list3) - 1]
  
    ##########################################################################################

    fs = open("name.txt","r")
    list_1 = []
    list_2 = [] 
    list_3 = []
    list_4 = []
    list_5 = []
    dic = {} 

    for c in fs:
        list_1.append(c)
    for d in range(0,len(list_1)):
        if d != len(list_1)-1:
           list_2.append(list_1[d][:len(list_1[d])-1])
        elif d == len(list_1) - 1:
             list_2.append(list_1[d])
    for e in list_2:
        l = []
        l = e.split("-")
        list_3.append(int(l[1]))
        dic[int(l[1])] = l[0]
        list_3.sort()      
    
    list_4 = list_3[::-1]
    for c in range(5):
        list_5.append(list_4[c])

    SplashScreen()
    play = instruction(score)
    while truth1 == True:
          if  play == True:
             
             game()
	     play = instruction(score)
             print "ppll"
	     print play
          elif play == False:
               print list_1
               print list_2
               print list_3
               print list_4
               print list_5
               print dic
               truth1 = False


def SplashScreen():
    screen = pygame.display.set_mode((640,480))
    splash =  Splash()      
    allsprites = pygame.sprite.LayeredUpdates(splash)

    clock = pygame.time.Clock()
    truth2 = True
    play = True

    while truth2 == True:
          clock.tick(30)
    
          for event in pygame.event.get():
              if event.type == pygame.QUIT:
                 truth2 = False
                 play = False
              elif event.type == pygame.MOUSEBUTTONDOWN:
		   play = True
                   truth2 = False
              elif event.type == pygame.KEYDOWN:
                   keyname = pygame.key.name(event.key)
                   if event.key == pygame.K_ESCAPE:
                      print keyname
                      play = False
                      truth2 = False
                   
          
          allsprites.update()
          allsprites.draw(screen)
	  pygame.display.flip()
         
      





def instruction(score):
    screen = pygame.display.set_mode((640,480))   
    ocean = Ocean()
    ship = Ship()
    
    list = ["Pirates Of Caribbean                High Score - %s" % (score),"","You are Captain Jack Sparrow.","You are on a quest to travel the seven seas,","but there many enemies in your way trying to stop you","Your ammo is limited but you can gain more ammo","by distroying the cargo ship.","Be aware of the RAF squadron","ALL THE BEST","","Click to Start and press ESC to exit"]
        
    allsprites = pygame.sprite.LayeredUpdates(ocean,ship)
    myfont = pygame.font.SysFont("none",30)
     
    clock = pygame.time.Clock()
    truth2 = True
    
    play = True
        
    while truth2 == True:
          clock.tick(30)
    
          for event in pygame.event.get():
              if event.type == pygame.QUIT:
                 truth2 = False
                 play = False
              elif event.type == pygame.MOUSEBUTTONDOWN:
                   play = True
                   truth2 = False
              elif event.type == pygame.KEYDOWN:
                   keyname = pygame.key.name(event.key)
                   if event.key == pygame.K_ESCAPE:
                      print keyname
                      play = False
                      truth2 = False
                   
          
          allsprites.update()
          allsprites.draw(screen)
          count = 1
          for c in list:     
              text = myfont.render(c,1,(255,0,0))
              screen.blit(text,(30,30*count))
              count += 1
          pygame.display.flip()
          
    return play   
        
               
def game():
    print "again"
    screen = pygame.display.set_mode((640,480))
    background = pygame.Surface(screen.get_size())
    background = background.convert()
    background.fill((0,0,255))
    screen.blit(background,(0,0))

    truth3 = True
    clock = pygame.time.Clock()
  
    ocean = Ocean()

    ship = Ship()
    ship.rect.inflate_ip((-20,-20))
    rocket_f1 = Rocket()

    enemy = Enemy()
    enemy.dy = 8
    rocket_e1 = ERocket()

    plane = Plane()
    rocket_e2 = Plane_rocket1()
    rocket_e3 = Plane_rocket2() 
    scoreboard = Scoreboard()
    
    
    cargo = Cargo()   
    cargo_r1 = Cargo_rocket1()   
    cargo_r2 = Cargo_rocket2()
    cargo_r3 = Cargo_rocket3()

    ammo = Ammo(cargo)

    scoreboard.center = (300,30)         

    friendlysprites = pygame.sprite.LayeredUpdates(ocean,ship,rocket_f1,scoreboard)
    enemysprites = pygame.sprite.LayeredUpdates(enemy,cargo)
    ammosprite = pygame.sprite.LayeredUpdates(ammo)
    planesprite = pygame.sprite.LayeredUpdates(plane)
    rocket_e1sprite = pygame.sprite.LayeredUpdates(rocket_e1)
    rocket_e2sprite = pygame.sprite.LayeredUpdates(rocket_e2,rocket_e3)
    cargo_rsprites = pygame.sprite.LayeredUpdates(cargo_r1,cargo_r2,cargo_r3)

    pygame.mouse.set_visible(False)   

    fire_f1 = False
    fire_e1 = False 
    fire_e2 = False
    fire_e3 = False
    fire_e4 = False
    fire_e5 = False
    fire_e6 = False
    fire = False  
 
    while truth3 == True:
          clock.tick(30)
 
          print scoreboard.rocket 
        
          ##############################################################################      
          rocket_f1.mx = ship.rect.centerx  #reset
          if fire_f1 == False: 
             rocket_f1.rect.center = ship.rect.center
          elif fire_f1 == True:
               rocket_f1.rect.centerx = mx #lock x coordinate
               rocket_f1.rect.centery -= 10
                
                  
          if rocket_f1.rect.bottom <= 0 :
             rocket_f1.reset()
             fire_f1 = False
          
          rocket_fkill = pygame.sprite.spritecollide(rocket_f1,enemysprites,False)
          if rocket_fkill :
             rocket_f1.reset()
             fire_f1 = False
             scoreboard.point += 100
             for c in rocket_fkill:
                 c.reset()
                 rocket_e1.reset(enemy)

          rocket_fkill2 = pygame.sprite.spritecollide(rocket_f1,planesprite,False)
          if rocket_fkill2:
             rocket_f1.reset()
             if fire_f1 == True:
                scoreboard.point += 200
             for c in rocket_fkill2:
                 if fire_f1 == True:
                    c.reset()
             fire_f1 = False
            
          
          ################################################################################
                             

          ################################################################################
          if enemy.rect.centerx in range(ship.rect.centerx - 40 , ship.rect.centerx + 40):
             fire_e1 = True
          if fire_e1 == True:
             rocket_e1.dy += 10

          if rocket_e1.rect.top >= screen.get_height():
             rocket_e1.reset(enemy)
             fire_e1 = False
             rocket_e1.dy = 0

          if ship.rect.colliderect(rocket_e1.rect) == True:
             rocket_e1.reset(enemy)
             scoreboard.life -= 1
             fire_e1 = False 
             rocket_e1.dy = 0
          ################################################################################


          ################################################################################
          if (plane.rect.centerx in range(ship.rect.centerx - 40 , ship.rect.centerx + 40)):
              fire_e2 = True
          if (plane.rect.centery in range(ship.rect.centery - 100, ship.rect.centery + 100)):
              fire_e2 = False 
             
          if fire_e2 == True:
             rocket_e2.dy += 12
 
          if rocket_e2.rect.bottom >= screen.get_height():
             rocket_e2.reset(plane)
             fire_e2 = False
             rocket_e2.dy = 0 
             
          if rocket_e2.rect.colliderect(ship.rect) == True:
             rocket_e2.reset(plane)
             if fire_e2 == True:
                scoreboard.life -= 1             
             fire_e2 = False
             rocket_e2.dy = 0
          ####################################################################################
           
              
          ####################################################################################
          if (plane.rect.centerx in range(ship.rect.centerx - 40 , ship.rect.centerx + 40)): 
              fire_e3 = True
          if (plane.rect.centery in range(ship.rect.centery - 100 , ship.rect.centery + 100)):
              fire_e3 = False                      

          if fire_e3 == True:
             rocket_e3.dy += 12
 
          if rocket_e3.rect.bottom >= screen.get_height():
             rocket_e3.reset(plane)
             fire_e3 = False
             rocket_e3.dy = 0 
             
          if rocket_e3.rect.colliderect(ship.rect) == True:
             rocket_e3.reset(plane) 
             if fire_e3 == True:
                scoreboard.life -= 1            
             fire_e3 = False
             rocket_e3.dy = 0
          ####################################################################################
          
           
          ####################################################################################
          if (cargo_r1.rect.centerx in range(ship.rect.centerx - 40, ship.rect.centerx + 40)):
             fire_e4 = True

          if fire_e4 == True:
             cargo_r1.dy += 15

          if cargo_r1.rect.top >= screen.get_height():
             cargo_r1.reset(cargo)
             fire_e4 = False
             cargo_r1.dy = 0

          if cargo_r1.rect.colliderect(ship.rect):
             if fire_e4 == True:
                scoreboard.life -= 1
             fire_e4 = False
             cargo_r1.dy = 0
             cargo_r1.reset(cargo)
          ###################################################################################
          

          ###################################################################################
          if (cargo_r2.rect.centerx in range(ship.rect.centerx - 40,ship.rect.centerx + 40)):
             fire_e5 = True

          if fire_e5 == True:
             cargo_r2.dy += 15

          if cargo_r2.rect.bottom > screen.get_height():
             cargo_r2.reset(cargo)
             fire_e5 = False
             cargo_r2.dy = 0
          
          if cargo_r2.rect.colliderect(ship.rect):
             if fire_e5 == True:
                scoreboard.life -= 1
             fire_e5 = False
             cargo_r2.dy = 0
             cargo_r2.reset(cargo)       
          #####################################################################################
          
             
          #####################################################################################
          if (cargo_r3.rect.centerx in range(ship.rect.centerx - 40, ship.rect.centerx + 40)):
             fire_e6 = True

          if fire_e6 == True:
             cargo_r3.dy += 15

          if cargo_r3.rect.bottom > screen.get_height():
             cargo_r3.reset(cargo)
             fire_e6 = False
             cargo_r3.dy = 0

          if cargo_r3.rect.colliderect(ship.rect):
             if fire_e6 == True:
                scoreboard.life -= 1
             fire_e6 = False
             cargo_r3.dy = 0
             cargo_r3.reset(cargo)
          ################################################################################

       
          ################################################################################
          if cargo.rect.colliderect(enemy.rect):
             enemy.reset()  
          ################################################################################
      

          ################################################################################
          for event in pygame.event.get():              
              if event.type == pygame.QUIT :
                 truth3 = False
              elif event.type == pygame.MOUSEBUTTONDOWN:
                   if fire_f1 == False and scoreboard.rocket > 0:
                      scoreboard.rocket -= 1  
                      fire_f1 = True
                   mx = rocket_f1.rect.centerx #lock x coordinate
          ################################################################################
          
 
          ################################################################################
          killship = pygame.sprite.spritecollide(ship,enemysprites,False)
          if killship :
             scoreboard.life -= 1
             for c in killship:
                 c.reset()
                 rocket_e1.reset(enemy)

          ammofill = pygame.sprite.spritecollide(ship,ammosprite,False)
          if ammofill :
             scoreboard.rocket += ammo.count
             for c in ammofill:
                 c.reset(cargo)
          ################################################################################
   
          if scoreboard.life <= 0:
             truth3 = False         

          score = scoreboard.point       
  
          friendlysprites.update()
          enemysprites.update()
          rocket_e1sprite.update(enemy)
          planesprite.update()
          rocket_e2sprite.update(plane)
          ammosprite.update(cargo)
          cargo_rsprites.update(cargo)
          friendlysprites.draw(screen)
          enemysprites.draw(screen)
          planesprite.draw(screen)
          rocket_e1sprite.draw(screen)
          rocket_e2sprite.draw(screen)
          ammosprite.draw(screen)
          cargo_rsprites.draw(screen)          
          pygame.display.flip()

    
    sc = open("score.txt","a")
    sc.write("\n%s"%(score))
    sc.close()
    Score(score)
    return True


if __name__ == "__main__":
   main(score)


   