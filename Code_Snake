import pygame
from pygame.locals import *
import random

'''Deuxième jeu créé avec pygame, commencé le 17 décembre 2017'''
'''Un énorme cap a été franchi le 26 décembre 2017 concernant la gestion des déplacements'''
'''28 décembre : création d'un menu et des règles du jeu, avec le logiciel GIMP pour les règles'''
'''2 janvier : possibilité de rejouer (j'ai bien galéré avant de me rendre compte que je ne réinitialisais pas la liste Snake à chaque fois)'''
'''3 janvier : possibilité de mettre le jeu en pause en appuyant sur la barre d'espace'''

######################################

'''Les paramètres constants'''

largeur,hauteur = 450,450
carre_x,carre_y = 15,15
taille_pomme_x,taille_pomme_y = 15,15
taille_mur_x,taille_mur_y = 15,15
deplacement = carre_x
x0,y0 = 100,100 # Position initiale du serpent

############################################

'''Initialisation de pygame, icône, titre et fenêtre de taille donnée par les paramètres hauteur et largeur'''

pygame.init()
icone = pygame.image.load('icone_1_snake.jpg')
pygame.display.set_icon(icone)
pygame.display.set_caption('Snake - Par Eliane Schwindenhammer')
fenetre = pygame.display.set_mode((largeur,hauteur))

###############################################

'''Chargement des images à utiliser et transformation à la taille voulue'''

p = pygame.image.load('apple.png').convert_alpha()
pomme = pygame.transform.scale(p,(taille_pomme_x,taille_pomme_y)) # Pomme rouge
carre_vert_bis = pygame.image.load('carre_vert.png').convert_alpha()
carre_vert = pygame.transform.scale(carre_vert_bis,(carre_x,carre_y)) # Corps du serpent
regles_1 = pygame.image.load('regles_1.png').convert_alpha()
regle_1 = pygame.transform.scale(regles_1,(largeur,hauteur)) # Règles du jeu
perdu = pygame.image.load('perdu.jpg').convert_alpha()
fin = pygame.transform.scale(perdu,(largeur,hauteur)) # Image de fin

#################################################

'''Tout ce qui concerne la page de menu'''

# Coordonnées du texte apparaissant sur le menu

debut_titre = [(20,150),(20,250),(20,350)] # Coordonnées du coin supérieur gauche des rectangles définissant les 'boutons' du menu
titres = ['Jouer','Comment jouer','Quitter'] # Titres du menu
taille_rect = [(130,40),(340,40),(160,45)] # Taille (longueur,largeur) des rectangles définissant les 'boutons' du menu

def afficher_menu():
    
    '''Fonction qui affiche le menu, en utilisant des rectangles de la même couleur que le fond, que l'on 'remplit' de texte'''
    
    fond_menu = pygame.Surface(fenetre.get_size()).convert()
    fond_menu.fill((20,148,20))
    fenetre.blit(fond_menu,(0,0))
    
    police = pygame.font.SysFont('ravie', 80)
    police_2 = pygame.font.SysFont('grease',40)
    
    titre = police.render('Snake', True, (0,0,0))
    
    pos = titre.get_rect() # Rectangle dans lequel le texte sera
    pos.centerx = fenetre.get_rect().centerx # Positions du rectangle
    pos.centery = 50

    fond_menu.blit(titre,pos)
    
    for i in range(len(titres)):
        titre_1 = police_2.render(titres[i],True,(0,0,0))
        rectangle = pygame.draw.rect(fond_menu,(20,148,20),Rect(debut_titre[i],taille_rect[i]))
        fond_menu.blit(titre_1,debut_titre[i])
        
    fenetre.blit(fond_menu,(0,0))
    pygame.display.flip()
    
###########################################################################
    
'''Quelques fonctions utiles'''

def collide(x1,x2,y1,y2,w1,w2,h1,h2):
    
    '''Fonction qui renvoie un booléen : True si le serpent touche la pomme, False sinon'''
    
    if x1+w1>x2 and x1<x2+w2 and y1+h1>y2 and y1<y2+h2:return True
    else:return False

def apple_in_bad_pos(xpomme,ypomme):
    
    '''Renvoie un booléen indiquant si la pomme que l'on s'apprête à placer aléatoirement se trouvera sur le serpent'''
    
    mauvaise_pos = False
    
    for i in Snake:
        if collide(i[0],xpomme,i[1],ypomme,carre_x,taille_pomme_x,carre_y,taille_pomme_y):mauvaise_pos = True
    return mauvaise_pos

def afficher_serpent():
    
    '''Fonction qui se contente d'afficher un carré vert aux coordonnées présentes dans la liste Snake'''

    for i in range(len(Snake)):fenetre.blit(carre_vert,Snake[i])
    pygame.display.flip()

def update_deplacement(Snake,touche,manger):
    
    '''Fonction qui ajoute une nouvelle tête au serpent, et qui lui coupe la queue seulement s'il ne vient pas de manger une pomme '''
    
    if touche == 'right':Snake.append((head[0]+deplacement,head[1])) # On ajoute une nouvelle tête selon la direction du serpent
    elif touche == 'left':Snake.append((head[0]-deplacement,head[1]))
    elif touche == 'up':Snake.append((head[0],head[1]-deplacement))
    else:Snake.append((head[0],head[1]+deplacement))
    
    if manger == False: # On ne lui coupe la queue que s'il ne vient pas de manger une pomme
        f = fond.subsurface(tail[0],tail[1],carre_x,carre_y) # On recouvre l'ancienne queue de fond
        fenetre.blit(f,tail)
    
        Snake.pop(0) # On lui retire son ancienne queue

def se_mord_la_queue():
    
    '''Renvoie un booléen indiquant si la tête du serpent touche une partie de son corps'''
    
    return Snake.count(head)>1
    
def touche_les_bords():
    
    '''Renvoie un booléen indiquant si la tête du serpent touche les bords'''
    
    au_bord = False
    if head[0]>largeur-carre_x/2 or head[0]<carre_x/2 or head[1]>hauteur-carre_y/2 or head[1]<carre_y/2:au_bord = True
    return au_bord

def accelere(points):
    
    '''Fonction qui 'gèle' le jeu avec une durée (en ms) de plus en plus courte au fur et à mesure que le serpent grandit,
    ce qui donne l'impression que le serpent 'accélère' au fur et à mesure du jeu'''
    
    if points<5:pygame.time.wait(55)
    elif points<10:pygame.time.wait(50)
    elif points<15:pygame.time.wait(45)
    elif points<25:pygame.time.wait(40)
    elif points<35:pygame.time.wait(35)
    elif points<60:pygame.time.wait(30)
    else:pygame.time.wait(25)

######################################################

'''Les instructions pour le jeu lui-même'''

rejouer = 1
accueil = 1
fin_du_jeu = 0


while rejouer:

    continuer = 1
    clic = 0
    regles_du_jeu = 0
    touche = 'right'
    points = 0
    pause = False

    '''Le serpent est défini par une liste de coordonnées ; la queue correspond au 1er élément et la tête au dernier
    On fait bien attention à le réinitiaiser à chaque fois qu'on rejoue'''
    
    Snake = [(x0,y0),(x0+carre_x,y0),(x0+2*carre_x,y0)]
    xpomme,ypomme = random.uniform(50,largeur-50),random.uniform(50,hauteur-50) # Coordonnées de la première pomme à s'afficher

    while continuer: # Boucle principale de jeu
        
        
        if fin_du_jeu:
            
            for event in pygame.event.get():
                
                if event.type == QUIT or (event.type == KEYDOWN and event.key == K_ESCAPE):
                    rejouer = 0
                    pygame.quit()
                    
                elif event.type == KEYDOWN and event.key == K_SPACE:
    
                    accueil = 1
                    fin_du_jeu = 0
                    print('On rejoue !')

        jeu = 1
        
        while accueil:
            
            afficher_menu() # Affichage du menu
            
            for event in pygame.event.get():
                
                if event.type == QUIT:
                    continuer,accueil,jeu = 0,0,0
                    pygame.quit()
                
                elif event.type == MOUSEBUTTONDOWN:
                    
                    sx,sy = event.pos[0],event.pos[1] # On récupère les coordonnées de la souris
                    for i in range(len(debut_titre)):
                        if sx >=debut_titre[i][0] and sx<=debut_titre[i][0]+taille_rect[i][0] and sy>=debut_titre[i][1] and sy<=debut_titre[i][1]+taille_rect[i][1]:
                            
                            if i == 0:clic,accueil = 1,0
                            elif i == 1:regles_du_jeu,accueil = 1,0
                            elif i == 2:
                                jeu,accueil,continuer = 0,0,0
                                pygame.quit()
                                
        if regles_du_jeu : # Si le joueur a cliqué sur 'règles du jeu'
            
            fenetre.blit(regle_1,(0,0))
            pygame.display.flip()
            
            for event in pygame.event.get():
                
                if event.type == KEYDOWN and event.key == K_SPACE:
                    clic = 1
                    regles_du_jeu = 0
    
                elif event.type == QUIT or (event.type == KEYDOWN and event.key == K_ESCAPE):
                    continuer,accueil,jeu = 0,0,0
                    pygame.quit()
        
        elif clic: # Si le joueur a cliqué sur 'Jouer' ou bien était dans les règles du jeu et a appuyé sur la barre d'espace:

            while jeu:
                
                while pause: # Tant qu'on est en pause, on ne fait rien que d'attendre la reprise
                    
                    for event in pygame.event.get():
                    
                        if event.type == KEYDOWN and event.key == K_SPACE:
                        
                            pause = False
                        
                        elif event.type == QUIT:
                            
                            pygame.quit()
                
                
                fond = pygame.Surface(fenetre.get_size()).convert()
                fond.fill((100,100,200))
                fenetre.blit(fond,(0,0))
                fenetre.blit(pomme,(xpomme,ypomme))
            
                afficher_serpent()
                pygame.display.flip()
            
                action = False
                manger = False
                tail = Snake[0]
                head = Snake[-1]
                
                if se_mord_la_queue() or touche_les_bords():
                    
                    continuer = 0
                    jeu = 0
                    accueil = 0
                    fenetre.blit(fin,(0,0))
                    pygame.display.flip()
                    fin_du_jeu = 1
                    print(points)
                    
                
                elif collide(head[0],xpomme,head[1],ypomme,carre_x,taille_pomme_x,carre_y,taille_pomme_y):
                    
                    manger = True
                    f = fond.subsurface((xpomme,ypomme,taille_pomme_x,taille_pomme_y))
                    fenetre.blit(f,(xpomme,ypomme)) # On fait disparaître la pomme
                    pygame.display.flip()
                    points += 1 # Le score augmente de 1
                    xpomme,ypomme = random.uniform(50,largeur-50),random.uniform(50,hauteur-50) # Et on en place une autre aléatoirement
                    
                    while apple_in_bad_pos(xpomme,ypomme):xpomme,ypomme = random.uniform(50,largeur-50),random.uniform(50,hauteur-50)
                    fenetre.blit(pomme,(xpomme,ypomme))
                    update_deplacement(Snake,touche,manger)
                    afficher_serpent()
                
                else:
                    
                    for event in pygame.event.get():
                        
                        if event.type == QUIT:
                            continuer = 0
                            pygame.quit()
                            
                        elif event.type == KEYDOWN and event.key == K_ESCAPE:
                            continuer = 0
                            pygame.quit()
                        
                        elif event.type == KEYDOWN and event.key == K_SPACE:
                            pause = True
                        
                        else:
                            
                            if event.type == KEYDOWN and event.key in (K_RIGHT,K_LEFT,K_UP,K_DOWN):
                                
                                if event.key == K_RIGHT and touche!='right' and touche !='left':touche = 'right'
                                elif event.key == K_LEFT and touche !='left' and touche!='right':touche = 'left'
                                elif event.key == K_UP and touche!='up' and touche!='down':touche = 'up'
                                elif event.key == K_DOWN and touche!='down' and touche!='up':touche = 'down'
                
                                update_deplacement(Snake,touche,manger)
                                afficher_serpent()
                                action = True
                            
                            elif event.type == KEYDOWN and event.key == K_SPACE:
                                pause = True
                            
                    if action == False and pause == False:
                        
                        update_deplacement(Snake,touche,manger)
                        afficher_serpent()
                
                accelere(points)

# Reste à voir : affichage des points, murs, obstacles, pommes bonus
