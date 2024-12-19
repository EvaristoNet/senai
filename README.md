
# senai
import pygame
from pygame.locals import *
import random
import time


tamanho_tela = width, height =(700,650)
estrada = int(width/1.6)
linha_estrada = int(width/80)
faixa_direita = width/2 + estrada/4
faixa_esquerda = width/2 - estrada/4
velocidade = 1


pygame.init()

corrida = True
screen = pygame.display.set_mode(tamanho_tela)
pygame.display.set_caption("Jogo SENAI - MAUA")
screen.fill((60, 220, 0))
pygame.display.update()

carro_1 = pygame.image.load("carro_1.png")
carro_1_loc = carro_1.get_rect()
carro_1_loc.center = faixa_direita, height*0.8

carro_2 = pygame.image.load("carro_2.png")
carro_2_loc = carro_2.get_rect()
carro_2_loc.center = faixa_esquerda, height*0.2

game_over = pygame.image.load("game_over.png")
game_over_loc = game_over.get_rect()
game_over_loc.center = width/2, height*0.5




rodadas = 0

pygame.mixer.init()
pygame.mixer.music.load("musica.mp3")
pygame.mixer.music.play()


while corrida:

    rodadas += 1
    if rodadas == 5000:
        velocidade += 0.15
        rodadas = 0

    carro_2_loc[1] += velocidade
    if carro_2_loc[1] > height:
          if random.randint(0, 1) == 0:
               carro_2_loc.center = faixa_direita, -200
          else:
               carro_2_loc.center = faixa_esquerda, -200

    if carro_1_loc[0] == carro_2_loc[0] and carro_2_loc[1] > carro_1_loc[1] - 250:
        screen.blit(game_over, game_over_loc)
        pygame.display.update()
        time.sleep(3)
        break


    for event in pygame.event.get():
        if event.type == QUIT:
            corrida = False
        if event.type == KEYDOWN:
            if event.key in [K_a, K_LEFT]:
                carro_1_loc = carro_1_loc.move([-int(estrada/2),0])
            if event.key in [K_d, K_RIGHT]:
                carro_1_loc = carro_1_loc.move([int(estrada/2), 0])

    pygame.draw.rect(
        screen,
        (50,50,50),
        (width/2-estrada/2,0, estrada, height))
    
    pygame.draw.rect(
        screen,
        (255,240,60),
        (width/2-linha_estrada/2,0, linha_estrada, height))
    
    pygame.draw.rect(
        screen,
        (255,255,255),
        (width/2-estrada/2+linha_estrada*2,0, linha_estrada, height))
    
    pygame.draw.rect(
        screen,
        (255,255,255),
        (width/2+estrada/2-linha_estrada*3,0,linha_estrada, height))
    

    screen.blit(carro_1, carro_1_loc)
    screen.blit(carro_2, carro_2_loc)



    pygame.display.update()




pygame.quit()



