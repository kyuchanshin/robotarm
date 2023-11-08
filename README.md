코드시작
import pygame
import numpy as np
import random
import os

pygame.init()
pygame.mixer.init()

current_path = os.path.dirname(__file__)
assets_path = os.path.join(current_path, 'soundfile')
sound = pygame.mixer.Sound(os.path.join(assets_path, 're.mp3'))
sound2 = pygame.mixer.Sound(os.path.join(assets_path, 'mi.mp3'))
sound3 = pygame.mixer.Sound(os.path.join(assets_path, 'si.mp3'))
pygame.display.set_caption("20191108 신규찬")

FPS = 60   # frames per second
RED = (255, 0, 0)

WINDOW_WIDTH = 1400
WINDOW_HEIGHT = 800

다각형 만드는 함수
def getRegularPolygon(nV, radius=1.):
    angle_step = 360. / nV 
    half_angle = angle_step / 2.
    vertices = []
    for k in range(nV):
        degree = angle_step * k 
        radian = np.deg2rad(degree + half_angle)
        x = radius * np.cos(radian)
        y = radius * np.sin(radian)
        vertices.append( [x, y] )
    #
    print("list:", vertices)
    vertices = np.array(vertices)
    print('np.arr:', vertices)
    return vertices
class myPolygon():
    def __init__(self, nvertices = 3, radius=70, color=(100,0,0), vel=[5.,0]):
        self.radius = radius
        self.nvertices = nvertices
        self.vertices = getRegularPolygon(self.nvertices, radius=self.radius)
        self.color = color
        self.color_org = color 
        self.angle = 0.
        self.angvel = np.random.normal(5., 7)
        self.position = np.array([0.,0]) #
        # self.position = self.vertices.sum(axis=0) # 2d array
        self.vel = np.array(vel)
        self.tick = 0
    def update(self,):
        self.tick += 1
        self.angle += self.angvel
        self.position += self.vel
        if self.position[0] >= WINDOW_WIDTH:
            self.vel[0] = -1. * self.vel[0]
        if self.position[0] < 0:
            self.vel[0] *= -1.
        if self.position[1] >= WINDOW_HEIGHT:
            self.vel[1] *= -1.
        if self.position[1] < 0:
            self.vel[1] *= -1
        return
    def draw(self, screen):
        R = Rmat(self.angle)
        points = self.vertices @ R.T + self.position
        pygame.draw.polygon(screen, self.color, points)

행렬 함수
cos, sin, 라디안 값 할당하고
Tmat : translation, 회전 축을 이동
Rmat : 회전변환




def Rmat(degree):
    rad = np.deg2rad(degree) 
    c = np.cos(rad)
    s = np.sin(rad)
    R = np.array([ [c, -s, 0],
                   [s,  c, 0], [0,0,1]])
    return R

def Tmat(tx, ty):
    Translation = np.array( [
        [1, 0, tx],
        [0, 1, ty],
        [0, 0, 1]
    ])
    return Translation


def main():
    pygame.init()

메인함수 정의하기
    screen = pygame.display.set_mode( (WINDOW_WIDTH, WINDOW_HEIGHT) )
    clock = pygame.time.Clock()
    xVel = 0
    yVel = 0
    jaVel1 = 0
    jaVel2 = 0
    jaVel3 = 0
    rSpeed = 8
    rotSpeed = 8
    w1 = 80
    h1 = 30
    w2 = 40
    h2 = 8
    px = WINDOW_WIDTH / 2 - w1/2
    py = WINDOW_HEIGHT - 10
    X1 = np.array([ [0,0], [w1, 0], [w1, h1], [0, h1] ])
    X2 = np.array([ [0,0], [w2, 0], [w2, h2], [0, h2] ])
    position = [px, py]
    theta = 0
    jointangle1 = 0
    jointangle2 = 0
    jointangle3 = 0
    jointangle4 = 90
    jointangle5 = -90
    jointangle6 = -90
    tick = 0
    done = False

    

while 문 형성
    while not done:
        tick +=2
        for event in pygame.event.get():
                if event.type == pygame.QUIT:
                    done = True
                elif event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_LEFT:
                        xVel = -rSpeed
                        sound3.play()
                    elif event.key == pygame.K_RIGHT:
                        xVel = rSpeed
                        sound3.play()
                    elif event.key == pygame.K_UP:
                        yVel = -rSpeed
                        sound3.play()
                    elif event.key == pygame.K_DOWN:
                        yVel = rSpeed 
                        sound3.play()
                    elif event.key == pygame.K_SPACE:
                        jointangle5 = -120
                        jointangle6 = -60
                        sound.set_volume(0.5)
                        sound.play()
                    elif event.key == pygame.K_z:
                        jaVel1 = -rotSpeed
                        sound2.play()
                    elif event.key == pygame.K_x:
                        jaVel1 = rotSpeed
                        sound2.play()
                    elif event.key == pygame.K_a:
                        jaVel2 = -rotSpeed
                        sound2.play()
                    elif event.key == pygame.K_s:
                        jaVel2 = rotSpeed
                        sound2.play()
                    elif event.key == pygame.K_q:
                        jaVel3 = -rotSpeed
                        sound2.play()
                    elif event.key == pygame.K_w:
                        jaVel3 = rotSpeed
                        sound2.play()

                elif event.type == pygame.KEYUP:
                    if event.key == pygame.K_LEFT or event.key == pygame.K_RIGHT:
                        xVel = 0
                        sound3.stop()
                    elif event.key == pygame.K_UP or event.key == pygame.K_DOWN:
                        yVel = 0   
                        sound3.stop()
                    elif event.key == pygame.K_SPACE:
                        jointangle5 = -90
                        jointangle6 = -90
                    elif event.key == pygame.K_z:
                        jaVel1 = 0
                    elif event.key == pygame.K_x:
                        jaVel1 = 0
                    elif event.key == pygame.K_a:
                        jaVel2 = 0
                    elif event.key == pygame.K_s:
                        jaVel2 = 0
                    elif event.key == pygame.K_q:
                        jaVel3 = 0
                    elif event.key == pygame.K_w:
                        jaVel3 = 0

움직임 지정

        position[0] += xVel
        position[1] += yVel
        jointangle1 += jaVel1
        jointangle2 += jaVel2
        jointangle3 += jaVel3

        print(jointangle1)
        
        theta += random.randrange(1,10)

        # drawing
        screen.fill((255, 255, 255))

        #initial position
        pygame.draw.circle(screen, (255,0,0), position, radius = 7)
        
        
시작점 할당
        #base
        H0 = Tmat(position[0], position[1]) @ Tmat(0, -h1)
        draw(X1, H0, screen, (0,0,0))

첫번째 팔       
        H1 = H0 @ Tmat(w1/2,0) 
        x, y = H1[0,2], H1[1,2] #joint1
        H11 = H1 @ Rmat(-90) @ Tmat(0, -h1/2)
        pygame.draw.circle(screen, (255,0,0), (x,y), radius = 7)
        H12 = H11 @ Tmat(0,h1/2) @ Rmat(jointangle1) @ Tmat(0, -h1/2)
        draw(X1, H12, screen, (0,0,0))

두번째 팔
        H2 = H12 @ Tmat(w1,0) @ Tmat(0, h1/2) #joint2
        x, y = H2[0,2], H2[1,2]
        pygame.draw.circle(screen, (255,0,0), (x,y), radius = 7)
        H21 = H2 @ Rmat(jointangle2) @ Tmat(0, -h1/2)
        draw(X1, H21, screen, (0,0,0))

세번째 팔
        H3 = H21 @ Tmat(w1,0) @ Tmat(0, h1/2) #joint3
        x, y = H3[0,2], H3[1,2]
        pygame.draw.circle(screen, (255,0,0), (x,y), radius = 7)
        H31 = H3 @ Rmat(jointangle3) @ Tmat(0, -h1/2)
        draw(X1, H31, screen, (0,0,0))

손 기초
        H4 = H31 @ Tmat(w1,0) @ Tmat(0, h1/2) #joint4
        x, y = H4[0,2], H4[1,2]
        pygame.draw.circle(screen, (0,0,0), (x,y), radius = 7)
        H41 = H4 @ Rmat(jointangle4) @ Tmat(-w2/2, -h2)
        draw(X2, H41, screen, (0,0,0))

오른쪽 손가락
        H5 = H41 @ Tmat(w2,0) @ Tmat(0, h2/2)
        x, y = H5[0,2], H5[1,2]
        pygame.draw.circle(screen, (255,0,0), (x,y), radius = 7)
        H51 = H5 @ Rmat(jointangle5) @ Tmat(0, -h2/2)
        draw(X2, H51, screen, (0,0,0))

왼쪽 손가락
        H6 = H41 @ Tmat(0, h2/2)
        x, y = H6[0,2], H6[1,2]
        pygame.draw.circle(screen, (255,0,0), (x,y), radius = 7)
        H61 = H6 @ Rmat(jointangle6) @ Tmat(0, -h2/2)
        draw(X2, H61, screen, (0,0,0))

        pygame.display.flip()
        clock.tick(FPS)
    # end of while
# end of main()

if __name__ == "__main__":
    main()
