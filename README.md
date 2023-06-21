俄罗斯方块简易代码



import pygame
import random

# 游戏窗口的宽度和高度
window_width = 400
window_height = 600

# 方块大小
block_size = 25

# 方块的颜色
colors = [
    (0, 0, 0),    # 黑色
    (255, 0, 0),  # 红色
    (0, 255, 0),  # 绿色
    (0, 0, 255),  # 蓝色
    (255, 255, 0) # 黄色
]

# 初始化Pygame
pygame.init()

# 创建游戏窗口
window = pygame.display.set_mode((window_width, window_height))
pygame.display.set_caption('俄罗斯方块')

clock = pygame.time.Clock()

# 方块类
class Block:
    def __init__(self, x, y, color):
        self.x = x
        self.y = y
        self.color = color

# 游戏主循环
def game_loop():
    # 生成一个新的方块
    def generate_new_block():
        x = random.randint(1, (window_width - block_size) // block_size) * block_size
        y = 0
        color = random.randint(1, len(colors) - 1)
        return Block(x, y, color)

    # 检查方块是否与其他方块重叠或超出边界
    def check_collision():
        for block in current_block:
            if block.x < 0 or block.x >= window_width or block.y < 0 or block.y >= window_height:
                return True
            for other_block in blocks:
                if block.x == other_block.x and block.y == other_block.y:
                    return True
        return False

    # 方块移动
    def move_block(dx, dy):
        for block in current_block:
            block.x += dx
            block.y += dy

    # 方块旋转
    def rotate_block():
        center_block = current_block[1]
        for block in current_block:
            x_relative = block.x - center_block.x
            y_relative = block.y - center_block.y
            block.x = center_block.x - y_relative
            block.y = center_block.y + x_relative

    # 方块下落
    def drop_block():
        while not check_collision():
            move_block(0, block_size)

    # 方块固定
    def fix_block():
        for block in current_block:
            blocks.append(Block(block.x, block.y, block.color))

    # 方块消除
    def clear_lines():
        full_lines = []
        for y in range(window_height // block_size):
            line_blocks = [block for block in blocks if block.y == y * block_size]
            if len(line_blocks) == window_width // block_size:
                full_lines.append(y)
        for y in full_lines:
            for block in blocks:
                if block.y < y * block_size:
                    block.y += block_size
                elif block.y == y * block_size:
                    blocks.remove(block)

    # 绘制方块
    def draw_block(block):
        pygame.draw.rect(window, colors[block.color], pygame.Rect(block.x, block.y, block_size
