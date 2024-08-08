# Convolucao em 1D

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
    }
}
```

# Convolucao em 2D
```java
import java.awt.GridLayout;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.IOException;
import javax.imageio.ImageIO;
import javax.swing.ImageIcon;
import javax.swing.JFrame;
import javax.swing.JLabel;

public class ImageConvolution {

    // Função para ler uma imagem em escala de cinza
    public static BufferedImage lerImagem(String caminho) throws IOException {
        BufferedImage imagemColorida = ImageIO.read(new File(caminho));
        BufferedImage imagemCinza = new BufferedImage(imagemColorida.getWidth(), imagemColorida.getHeight(), BufferedImage.TYPE_BYTE_GRAY);
        imagemCinza.getGraphics().drawImage(imagemColorida, 0, 0, null);
        return imagemCinza;
    }

    // Função que realiza a convolução 2D
    public static double[][] convoluir(double[][] imagem, double[][] kernel) {
        int larguraImagem = imagem[0].length;
        int alturaImagem = imagem.length;
        int larguraKernel = kernel[0].length;
        int alturaKernel = kernel.length;
        int novaLargura = larguraImagem - larguraKernel + 1;
        int novaAltura = alturaImagem - alturaKernel + 1;
        double[][] imagemFiltrada = new double[novaAltura][novaLargura];

        for (int i = 0; i < novaAltura; i++) {
            for (int j = 0; j < novaLargura; j++) {
                double soma = 0.0;
                for (int k = 0; k < alturaKernel; k++) {
                    for (int l = 0; l < larguraKernel; l++) {
                        soma += imagem[i + k][j + l] * kernel[k][l];
                    }
                }
                imagemFiltrada[i][j] = soma;
            }
        }

        return imagemFiltrada;
    }

    // Função para converter BufferedImage para matriz 2D de pixels
    public static double[][] imagemParaMatriz(BufferedImage imagem) {
        int largura = imagem.getWidth();
        int altura = imagem.getHeight();
        double[][] matriz = new double[altura][largura];
        for (int y = 0; y < altura; y++) {
            for (int x = 0; x < largura; x++) {
                int argb = imagem.getRGB(x, y);
                int intensidade = argb & 0xff; // Extrai o valor de intensidade (grayscale)
                matriz[y][x] = intensidade;
            }
        }
        return matriz;
    }

    // Função para salvar a matriz 2D de pixels em uma imagem
    public static BufferedImage matrizParaImagem(double[][] matriz) {
        int altura = matriz.length;
        int largura = matriz[0].length;
        BufferedImage imagem = new BufferedImage(largura, altura, BufferedImage.TYPE_BYTE_GRAY);
        for (int y = 0; y < altura; y++) {
            for (int x = 0; x < largura; x++) {
                int valor = (int) Math.min(Math.max(matriz[y][x], 0), 255);
                int argb = (0xff << 24) | (valor << 16) | (valor << 8) | valor;
                imagem.setRGB(x, y, argb);
            }
        }
        return imagem;
    }

    // Função para exibir as imagens em uma janela
    public static void exibirImagens(BufferedImage original, BufferedImage suavizada, BufferedImage bordas) {
        // Cria uma janela
        JFrame frame = new JFrame("Exibição de Imagens");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setLayout(new GridLayout(1, 3)); // Layout para exibir três imagens lado a lado

        // Adiciona a imagem original
        JLabel labelOriginal = new JLabel(new ImageIcon(original));
        labelOriginal.setText("Original");
        labelOriginal.setHorizontalTextPosition(JLabel.CENTER);
        labelOriginal.setVerticalTextPosition(JLabel.BOTTOM);
        frame.add(labelOriginal);

        // Adiciona a imagem suavizada
        JLabel labelSuavizada = new JLabel(new ImageIcon(suavizada));
        labelSuavizada.setText("Suavizada");
        labelSuavizada.setHorizontalTextPosition(JLabel.CENTER);
        labelSuavizada.setVerticalTextPosition(JLabel.BOTTOM);
        frame.add(labelSuavizada);

        // Adiciona a imagem de bordas
        JLabel labelBordas = new JLabel(new ImageIcon(bordas));
        labelBordas.setText("Bordas");
        labelBordas.setHorizontalTextPosition(JLabel.CENTER);
        labelBordas.setVerticalTextPosition(JLabel.BOTTOM);
        frame.add(labelBordas);

        // Configura a janela e exibe
        frame.pack();
        frame.setVisible(true);
    }

    public static void main(String[] args) {
        try {
            // Caminho para o arquivo de imagem
            String caminhoImagem = "C:\\Users\\gabri\\Downloads\\College\\Engenharia certa\\Sistema e Sinais\\Convoluçao\\220px-Joseph_Fourier.jpg";

            // Leitura da imagem
            BufferedImage imagemOriginal = lerImagem(caminhoImagem);

            // Conversão para matriz 2D
            double[][] matrizImagem = imagemParaMatriz(imagemOriginal);

            // Definição dos kernels
            double[][] kernelSuavizacao = {
                {1 / 9.0, 1 / 9.0, 1 / 9.0},
                {1 / 9.0, 1 / 9.0, 1 / 9.0},
                {1 / 9.0, 1 / 9.0, 1 / 9.0}
            };

            double[][] kernelBordas = {
                {-1, -1, -1},
                {-1,  8, -1},
                {-1, -1, -1}
            };

            // Aplicação do kernel de suavização
            double[][] imagemSuavizada = convoluir(matrizImagem, kernelSuavizacao);

            // Aplicação do kernel de detecção de bordas
            double[][] imagemBordas = convoluir(matrizImagem, kernelBordas);

            // Conversão das matrizes resultantes para imagens
            BufferedImage imagemSuavizadaFinal = matrizParaImagem(imagemSuavizada);
            BufferedImage imagemBordasFinal = matrizParaImagem(imagemBordas);

            // Exibe as imagens na tela
            exibirImagens(imagemOriginal, imagemSuavizadaFinal, imagemBordasFinal);

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```