# gitTutorial
#Импорт бибилиотеки
import pygame
#Инициализация библиотеки
pygame.init()
#Создание окна и его размеров
win = pygame.display.set_mode((500, 500))
#Название окна
pygame.display.set_caption("SemPunk_Game_Launch")

#Загрузка изображений для персонажа
walkRight = [pygame.image.load('right_1.png'), 
pygame.image.load('right_2.png'), 
pygame.image.load('right_3.png'), 
pygame.image.load('right_4.png'), 
pygame.image.load('right_5.png'), 
pygame.image.load('right_6.png')]

walkLeft = [pygame.image.load('left_1.png'), 
pygame.image.load('left_2.png'), 
pygame.image.load('left_3.png'), 
pygame.image.load('left_4.png'), 
pygame.image.load('left_5.png'), 
pygame.image.load('left_6.png')]

bg = pygame.image.load('bg.jpg')
playerStand = pygame.image.load('idle.png')

#Переменная для фреймов
clock = pygame.time.Clock()

#Праметры Персонажа
x = 50
y = 420
width = 60
height = 71
speed = 10

#Переменные для отслеживание прыжка персонажа
isjump = False
jumpcount = 10

#Вставка переменных для картинки
leftCount = False
rightCount = False
animCount = 0

#Переменная куда смотрит персонаж
lastMove = "right"

#Конструктор класса анимация снарядов полёт радиус размер и т.д
class weapone():
	def __init__(self, x, y, radius, color, facing):
		self.x = x
		self.y = y
		self.radius = radius
		self.color = color
		self.facing = facing
		self.vel = 8 * facing
		
		
	#функция отрисовки снаряда
	def draw(self, win):
		pygame.draw.circle(win, self.color, (self.x, self.y), self.radius)



#функция Обновления изображений персонажа и бэкграунда окна.
def drawWindow():
	global animCount
	win.blit(bg, (0, 0))

	if animCount + 1 >= 30:
		animCount = 0

	if leftCount:
		win.blit(walkLeft [animCount // 5], (x, y))
		animCount += 1
	elif rightCount:
		win.blit(walkRight [animCount // 5], (x, y))
		animCount += 1
	else:
		win.blit(playerStand, (x, y))

	for bullet in bullets:
		bullet.draw(win)

	
	pygame.display.update()


#Запуск игры
run = True
bullets = []
while run:
	clock.tick(30) #Обновление окна(скорость обновления)
	

	for event in pygame.event.get():
		if	event.type == pygame.QUIT:
			run = False
#Анимация полета снаряда b его исчезновение
	for bullet in bullets:
		if bullet.x < 500 and bullet.x > 0:
			bullet.x += bullet.vel
		else:
			bullets.pop(bullets.index(bullet))

#Управление и движение персонажа и его расположение в окне
	keys = pygame.key.get_pressed()


	if keys[pygame.K_f]:
		if lastMove == "right":
			facing = 1
		else:
			facing = -1

		if len(bullets) < 5:
			bullets.append(weapone(round(x + width //2), round(y + height // 2), 5, (255,0,0), facing))

	if keys [pygame.K_LEFT] and x > 5:
		x -= speed
		leftCount = True
		rightCount = False
		lastMove = "left"
	elif keys [pygame.K_RIGHT] and x < 500 - width - 5:
		x += speed
		leftCount = False
		rightCount = True
		lastMove = "right"
	else:
		leftCount = False
		rightCount = False
		animCount = 0
	#Движение с прыжком	
	if not(isjump):	
		if keys [pygame.K_SPACE]:
			isjump = True
	else:
		if jumpcount >= -10:
			if jumpcount < 0:
				y += (jumpcount ** 2) / 2
			else:
				y -= (jumpcount ** 2) / 2
			jumpcount -= 1
		else:
			isjump = False
			jumpcount = 10

	drawWindow()		



pygame.quit()
