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

   




    n = np.zeros(num_amostras)
    for i in range(num_amostras):
        n[i] = i
  
    plt.plot(n,s, color='b')
    plt.plot(n,x, color='r', linestyle='dotted')
    
    n = np.zeros(tam_saida)
    for i in range(tam_saida):
        n[i] = i
    
    plt.plot(n,y, color='g')
    plt.show()


if __name__ == "__main__":
    main()
```