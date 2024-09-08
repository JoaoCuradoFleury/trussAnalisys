import pygame
from flask import Flask, Response
import io

# Inicializa o Pygame
pygame.init()

# Configurações da janela
width, height = 800, 600
screen = pygame.display.set_mode((width, height))
pygame.display.set_caption("Bola Pula-Pula")

# Cor da bola
ball_color = (255, 0, 0)

# Posição inicial da bola
ball_x = width // 2
ball_y = height // 2
ball_radius = 20

# Velocidade da bola
ball_speed_x = 5
ball_speed_y = 5

# Cria um objeto Flask
app = Flask(__name__)

# Função para capturar o frame do jogo e enviá-lo como uma resposta HTTP
def generate_frames():
    while True:
        # Limpa a tela
        screen.fill((255, 255, 255))

        # Move a bola
        ball_x += ball_speed_x
        ball_y += ball_speed_y

        # Rebate a bola nas bordas
        if ball_x + ball_radius > width or ball_x - ball_radius < 0:
            ball_speed_x *= -1
        if ball_y + ball_radius > height or ball_y - ball_radius < 0:
            ball_speed_y *= -1

        # Desenha a bola
        pygame.draw.circle(screen, ball_color, (ball_x, ball_y), ball_radius)

        # Atualiza a tela
        pygame.display.flip()

        # Captura o frame atual
        frame = pygame.image.tostring(screen, 'RGB')

        # Converte o frame para um formato que o Flask possa enviar
        yield (b'--frame\r\n'
               b'Content-Type: image/png\r\n\r\n' + frame + b'\r\n')

# Rota para servir o vídeo
@app.route('/video_feed')
def video_feed():
    return Response(generate_frames(), mimetype='multipart/x-mixed-replace; boundary=frame')

if __name__ == '__main__':  

    # Inicia o servidor Flask em segundo plano
    from threading import Thread
    thread = Thread(target=app.run)
    thread.start()

    # Loop principal do jogo
    running = True
    while running:
        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                running = False  


    pygame.quit()
