import pygame
import math
import random
from pygame.locals import *

# 初始化
pygame.init()
pygame.mixer.init()

# 窗口设置
WIDTH, HEIGHT = 1920, 1080
screen = pygame.display.set_mode((WIDTH, HEIGHT), FULLSCREEN)
pygame.display.set_caption("永恒之爱 - 刘宇航 ♥ 李爽")

# 验证姓名
def validate_names():
    try:
        with open("names.config", "r") as f:
            names = f.read().split(",")
            if names != ["刘宇航", "李爽"]:
                pygame.quit()
                exit()
    except:
        pygame.quit()
        exit()

validate_names()

# 加载资源
heart_img = pygame.image.load("heart.png").convert_alpha()
bg_music = pygame.mixer.Sound("love.mp3")

# 颜色定义
PINK_GRADIENT = [(255, 192+i, 203+i) for i in range(0, 55, 5)]
GOLD = (255, 215, 0)

# 粒子系统
class Particle:
    def __init__(self, pos):
        self.pos = list(pos)
        self.velocity = [random.uniform(-1, 1), random.uniform(-5, 0)]
        self.timer = random.randint(20, 40)
        self.color = random.choice(PINK_GRADIENT)

    def update(self):
        self.pos[0] += self.velocity[0]
        self.pos[1] += self.velocity[1]
        self.timer -= 1
        return self.timer > 0

# 分形树参数
class FractalTree:
    def __init__(self):
        self.growth_speed = 0
        self.branches = []
        self.flowers = []
        self.angle = math.pi/4

    def generate_branch(self, start, length, angle, depth):
        if depth > 10 or length < 2:
            return
        end = (
            start[0] + length * math.cos(angle),
            start[1] - length * math.sin(angle)
        )
        self.branches.append((start, end, length))
        self.generate_branch(end, length*0.7, angle + self.angle, depth+1)
        self.generate_branch(end, length*0.7, angle - self.angle, depth+1)
        
        # 添加花朵
        if depth > 5 and random.random() < 0.3:
            self.flowers.append(end)

# 心动动画类
class Heartbeat:
    def __init__(self):
        self.size = 1.0
        self.growing = True

    def update(self):
        if self.growing:
            self.size += 0.02
            if self.size > 1.5:
                self.growing = False
        else:
            self.size -= 0.02
            if self.size < 1.0:
                self.growing = True
        return self.size

# 主程序
def main():
    # 初始化变量
    tree = FractalTree()
    particles = []
    hearts = []
    start_time = pygame.time.get_ticks()
    hb_effect = Heartbeat()
    bg_music.play(loops=-1)

    # 主循环
    running = True
    while running:
        screen.fill((25, 25, 50))  # 深空背景

        # 事件处理
        for event in pygame.event.get():
            if event.type == QUIT or (event.type == KEYDOWN and event.key == K_ESCAPE):
                running = False

        # 生成分形树
        if tree.growth_speed < 1:
            tree.growth_speed += 0.001
            tree.angle = math.pi/4 * tree.growth_speed
            tree.branches = []
            tree.generate_branch((WIDTH/2, HEIGHT-50), 150*tree.growth_speed, math.pi/2, 0)

        # 绘制树
        for branch in tree.branches:
            width = max(1, branch[2]/10)
            pygame.draw.line(screen, (139,69,19), branch[0], branch[1], int(width))

        # 绘制花朵
        for pos in tree.flowers:
            radius = random.randint(3,6)
            pygame.draw.circle(screen, PINK_GRADIENT[-1], pos, radius)

        # 花瓣飘落
        if random.random() < 0.3:
            particles.append(Particle((random.randint(0, WIDTH), 0)))

        # 更新粒子
        particles = [p for p in particles if p.update()]
        for p in particles:
            pygame.draw.circle(screen, p.color, (int(p.pos[0]), int(p.pos[1])), 2)

        # 生成心动效果
        if len(hearts) < 30:
            hearts.append((
                random.randint(0, WIDTH),
                random.randint(0, HEIGHT),
                random.randint(20,40),
                random.randint(0,100)
            ))

        # 绘制动态心形
        scale = hb_effect.update()
        for i, (x,y,s,phase) in enumerate(hearts):
            alpha = abs(math.sin((pygame.time.get_ticks()+phase)*0.005)) * 255
            scaled_size = int(s * (1 + 0.2 * math.sin(pygame.time.get_ticks()*0.005)))
            heart = pygame.transform.smoothscale(heart_img, (scaled_size, scaled_size))
            heart.set_alpha(int(alpha))
            screen.blit(heart, (x-scaled_size//2, y-scaled_size//2))

        # 绘制文字信息
        time_passed = (pygame.time.get_ticks() - start_time) // 1000
        text = pygame.font.SysFont("华文行楷", 60).render(
            f"刘宇航 ♥ 李爽 - 相爱{time_passed}秒", True, GOLD)
        screen.blit(text, (WIDTH//2 - text.get_width()//2, 50))

        date_text = pygame.font.SysFont("simhei", 40).render(
            "2023年3月9日 命运相遇", True, (255,255,255))
        screen.blit(date_text, (WIDTH//2 - date_text.get_width()//2, HEIGHT-100))

        # 绘制进度条
        pygame.draw.rect(screen, (100,100,100), (WIDTH//2-150, HEIGHT-80, 300, 20))
        pygame.draw.rect(screen, PINK_GRADIENT[5], (WIDTH//2-150, HEIGHT-80, 300*tree.growth_speed, 20))

        pygame.display.flip()
        pygame.time.delay(10)

    pygame.quit()

if __name__ == "__main__":
    main()
