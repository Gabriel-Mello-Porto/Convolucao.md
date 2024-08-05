# Convolucao 1D

# Graficos no doc
(https://docs.google.com/spreadsheets/d/1cK6e0UM3dsao9MdpZ-j0a601xRuQ7RKH49ijTCTXsM0/edit?usp=sharing)

```java
import java.util.Random;

public class App {
    public static void main(String[] args) {    
        
        // Parametro da frequencia de corte e numero de amostras
        int numAmostras = 50;
        double a = 0.5;

        // Entrada e saida
        double[] ruido = new double[numAmostras];
        double[] s = new double[numAmostras];
        double[] x = new double[numAmostras];
     
        // Impulso
        double[] h = new double[numAmostras];
        double[] h_Invertido = new double[numAmostras];     


        // Calcula iterativamente a resposta ao impulso e inverte o vetor
        for (int n = 0; n < numAmostras; n++) {
            h[n] = (1 - a) * Math.pow(a, n);
            //System.out.printf("\n%.5f", h[n]);
        }

        for (int i = 0; i < h.length; i++) {
            h_Invertido[i] = h[h.length - i - 1];
        }


        // Calcula iterativamente s[n]
        for (int n = 0; n < numAmostras; n++) {
            s[n] = (Math.sin(0.05 * n)) + (0.5 * Math.sin(0.15 * n));
        }

        // Calcula o ruido
        Random numAleatorio = new Random();
        for (int n = 0; n < numAmostras; n++) {
            ruido[n] = numAleatorio.nextDouble(-0.2, 0.2); // Amplitude máxima 0.2
        }

        // Gerar o sinal corrompido x[n]
        for (int n = 0; n < numAmostras; n++) {
            x[n] = s[n] + ruido[n];
            //x[n] = s[n];
        }


        // Calcular a convolução para obter o sinal filtrado y[n]
        int tamSaida = x.length + h.length - 1; // Tamanho do vetor resultante
        double[] y = new double[tamSaida];
        for (int i = 0; i < tamSaida; i++) {
            y[i] = 0;
            for (int j = 0; j < h.length; j++) {
                if (i - j >= 0 && i - j < x.length) {
                    y[i] += x[i - j] * h[j];
                }
            }
        }

        // Exibir todos os resultados
        System.out.println("\nn\ts[n]\tr[n]\tx[n]\th[n]\ty[n]");
        for (int n = 0; n < numAmostras; n++) {
            System.out.printf("%d\t%.3f\t%.3f\t%.3f\t%.3f\t%.3f\n", n, s[n], ruido[n], x[n], h_Invertido[n], y[n]);
        }

        for (int n = tamSaida - numAmostras + 1; n < tamSaida; n++) {
            System.out.printf("%d\t                                %.3f\n", n, y[n]);
        }

        
        /*
        System.out.println("\n\n");
        for (int n = 0; n < numAmostras; n++) {
            System.out.printf("\t%.5f\n", s[n]);
        }
        

        System.out.println("\n\n");
        for (int n = 0; n < numAmostras; n++) {
            System.out.printf("\t%.6f\n", x[n]);
        }

        System.out.println("\n\n");
        for (int n = 0; n < tamSaida; n++) {
            System.out.printf("\t%.6f\n", y[n]);
        }
        */
        
    }
}
```
# Convolucao 4x4 (exemplo simples)
```java
import java.util.Random;

public class App {
    public static void main(String[] args) {    
        

        // Parametro da frequencia de corte
        int numAmostras = 4;
        double a = 0.995;  
        
     
        // Impulso
        //double[] h = new double[numAmostras];
        double[] h_Invertido = new double[numAmostras];
        
        // Entrada e saida
        double[] ruido = new double[numAmostras];
        //double[] s = new double[numAmostras];
        double[] x = new double[numAmostras];
        //double[] y = new double[numAmostras];
        double[] s = {2, 4, 6, 8};
        double[] h = {3, 5, 7, 9};


        /*
        // Calcula iterativamente a resposta ao impulso e inverte o vetor
        for (int n = 0; n < numAmostras; n++) {
            h[n] = (1 - a) * Math.pow(a, n);
            //System.out.printf("\n%.5f", h[n]);
        }*/

        for (int i = 0; i < h.length; i++) {
            h_Invertido[i] = h[h.length - i - 1];
        }

        // Calcula o ruido
        Random numAleatorio = new Random();
        for (int n = 0; n < numAmostras; n++) {
            ruido[n] = numAleatorio.nextDouble(0.2); // Amplitude máxima 0.2
        }

        // Gerar o sinal corrompido x[n]
        for (int n = 0; n < numAmostras; n++) {
            //x[n] = s[n] + ruido[n];
            x[n] = s[n];
        }

        /*
        // Calcular a convolução para obter o sinal filtrado y[n]
        for (int n = 0; n < numAmostras; n++) {
            for (int m = 0; m <= n; m++) {
                y[n] += x[m] * h_Invertido[n - m];
            }
        }*/

        int tamSaida = x.length + h.length - 1; // Tamanho do vetor resultante
        double[] y = new double[tamSaida];
        for (int i = 0; i < tamSaida; i++) {
            y[i] = 0;
            for (int j = 0; j < h.length; j++) {
                if (i - j >= 0 && i - j < x.length) {
                    y[i] += x[i - j] * h_Invertido[j];
                }
            }
        }

        // Exibir todos os resultados
        System.out.println("\nn\ts[n]\tr[n]\tx[n]\th[n]\ty[n]");
        for (int n = 0; n < numAmostras; n++) {
            System.out.printf("%d\t%.3f\t%.3f\t%.3f\t%.3f\t%.3f\n", n, s[n], ruido[n], x[n], h_Invertido[n], y[n]);
        }

        for (int n = tamSaida - numAmostras + 1; n < tamSaida; n++) {
            System.out.printf("%d\t                                %.2f\n", n, y[n]);
        }

    }
}
```

# Convolucao com os graficos do python
(https://trinket.io/python3/0f15088021)

```python
import numpy as np
from matplotlib import pyplot as plt

def main():
    # Parametro da frequencia de corte e numero de amostras
    num_amostras = 200
    a = 0.5

    # Entrada e saida
    ruido = np.zeros(num_amostras)
    s = np.zeros(num_amostras)
    x = np.zeros(num_amostras)

    # Impulso
    h = np.zeros(num_amostras)
    h_invertido = np.zeros(num_amostras)

    # Calcula iterativamente a resposta ao impulso e inverte o vetor
    for n in range(num_amostras):
        h[n] = (1 - a) * (a ** n)

    for i in range(len(h)):
        h_invertido[i] = h[len(h) - i - 1]

    # Calcula iterativamente s[n]
    for n in range(num_amostras):
        s[n] = (np.sin(0.05 * n)) + (0.5 * np.sin(0.15 * n))

    # Calcula o ruido
    ruido = np.random.uniform(-0.2, 0.2, num_amostras)

    # Gerar o sinal corrompido x[n]
    x = s + ruido

    # Calcular a convolução para obter o sinal filtrado y[n]
    tam_saida = len(x) + len(h) - 1  # Tamanho do vetor resultante
    y = np.zeros(tam_saida)
    for i in range(tam_saida):
        y[i] = 0
        for j in range(len(h)):
            if i - j >= 0 and i - j < len(x):
                y[i] += x[i - j] * h[j]

    # Exibir todos os resultados
    print("\nn\ts[n]\tr[n]\tx[n]\th[n]\ty[n]")
    for n in range(num_amostras):
        print(f"{n}\t{s[n]:.3f}\t{ruido[n]:.3f}\t{x[n]:.3f}\t{h_invertido[n]:.3f}\t{y[n]:.3f}")

    for n in range(tam_saida - num_amostras + 1, tam_saida):
        print(f"{n}\t                                {y[n]:.3f}")

   



    # Graficos
    n = np.zeros(num_amostras)
    for i in range(num_amostras):
        n[i] = i
  
    plt.plot(n,s, color='b')
    plt.plot(n,x, color='r', linestyle='dotted')
    
    n = np.zeros(tam_saida)
    for i in range(tam_saida):
        n[i] = i
    
    plt.plot(n,y, color='g')

    plt.title('Azul = s[n]    Vermelho = x[n]     Verde = y[n]    a = 0.5')
    plt.xlabel('Amostras')
    plt.ylabel('Sinal')
    plt.grid(True)
    plt.xlim(0, 400)
    plt.show()


if __name__ == "__main__":
    main()
```

# 2D (not it)
```java
import javax.swing.*;
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.IOException;
import javax.imageio.ImageIO;

public class ImageConvolution {

    public static void main(String[] args) {
        try {
            // Carregar a imagem em escala de cinza
            BufferedImage image = ImageIO.read(new File("C:\\Users\\Larissa\\Downloads\\Hacker da Guatemala\\Sistemas e Sinais\\ImageHandling\\Joseph Fourier.jpg"));
            int width = image.getWidth();
            int height = image.getHeight();

            // Definir os kernels
            int[][] blurKernel = {
                {1, 1, 1},
                {1, 1, 1},
                {1, 1, 1}
            };
            int blurKernelSum = 9; // Soma dos valores do kernel de suavização

            int[][] edgeDetectionKernel = {
                {-1, -1, -1},
                {-1,  8, -1},
                {-1, -1, -1}
            };
            int edgeDetectionKernelSum = 1; // Não há necessidade de normalizar o kernel de detecção de bordas

            // Criar imagens para armazenar os resultados
            BufferedImage blurImage = new BufferedImage(width, height, BufferedImage.TYPE_BYTE_GRAY);
            BufferedImage edgeImage = new BufferedImage(width, height, BufferedImage.TYPE_BYTE_GRAY);

            // Aplicar o filtro de suavização
            applyFilter(image, blurImage, blurKernel, blurKernelSum);

            // Aplicar o filtro de detecção de bordas
            applyFilter(image, edgeImage, edgeDetectionKernel, edgeDetectionKernelSum);

            // Exibir as imagens lado a lado
            JFrame frame = new JFrame("Imagem Original, Suavização e Detecção de Bordas");
            frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
            frame.setSize(width * 3, height);

            // Crie um JPanel com GridLayout para exibir três imagens lado a lado
            JPanel panel = new JPanel(new GridLayout(1, 3));

            // Adicione a imagem original ao painel
            ImageIcon originalIcon = new ImageIcon(image);
            JLabel originalLabel = new JLabel(originalIcon);
            panel.add(originalLabel);

            // Adicione a imagem com suavização ao painel
            ImageIcon blurIcon = new ImageIcon(blurImage);
            JLabel blurLabel = new JLabel(blurIcon);
            panel.add(blurLabel);

            // Adicione a imagem com detecção de bordas ao painel
            ImageIcon edgeIcon = new ImageIcon(edgeImage);
            JLabel edgeLabel = new JLabel(edgeIcon);
            panel.add(edgeLabel);

            // Adicione o painel ao frame
            frame.add(panel);
            frame.setVisible(true);

            // Salvar as imagens resultantes
            ImageIO.write(blurImage, "jpg", new File("imagem_suavizacao.jpg"));
            ImageIO.write(edgeImage, "jpg", new File("imagem_deteccao_bordas.jpg"));

        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static void applyFilter(BufferedImage image, BufferedImage resultImage, int[][] kernel, int kernelSum) {
        int width = image.getWidth();
        int height = image.getHeight();
        int kernelSize = kernel.length;
        int offset = kernelSize / 2;

        for (int y = 0; y < height; y++) {
            for (int x = 0; x < width; x++) {
                int newValue = applyKernel(image, kernel, kernelSum, x, y);
                // Aplicar limitador para a imagem de detecção de bordas
                if (kernelSum == 1) { // Para detecção de bordas
                    newValue = Math.min(255, Math.max(0, newValue));
                }
                int rgb = (newValue << 16) | (newValue << 8) | newValue;
                resultImage.setRGB(x, y, rgb);
            }
        }
    }

    private static int applyKernel(BufferedImage image, int[][] kernel, int kernelSum, int x, int y) {
        int sum = 0;
        int kernelSize = kernel.length;
        int offset = kernelSize / 2;

        // Iterar sobre o kernel e aplicar a convolução
        for (int ky = 0; ky < kernelSize; ky++) {
            for (int kx = 0; kx < kernelSize; kx++) {
                // Verificar se o pixel está dentro dos limites da imagem
                int px = x + kx - offset;
                int py = y + ky - offset;
                if (px >= 0 && px < image.getWidth() && py >= 0 && py < image.getHeight()) {
                    int pixel = image.getRGB(px, py);
                    int gray = (pixel >> 16) & 0xFF; // Assumindo imagem em escala de cinza
                    sum += gray * kernel[ky][kx];
                }
            }
        }

        // Normalizar pelo valor da soma do kernel
        return sum / kernelSum;
    }
}
```