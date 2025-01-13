```
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text.Json;
using System.Threading.Channels;

2 - Função para verificar se o número pertence à sequência de Fibonacci
class Program
{
    static void Main(string[] args)
    {
        Console.WriteLine("Digite um numero: ");
        int numero = Convert.ToInt32(Console.ReadLine());

        if (IsFibonacci(numero))
        {
            Console.WriteLine($"O numero {numero} pertence à sequência de Fibonacci.");
        }
        else
        {
            Console.WriteLine($"O numero {numero} não pertence à sequência de Fibonacci.");
        }

        // 3 - Leitura e processamento dos dados do JSON
        string caminhoArquivo = "C:\\Users\\source\\repos\\Teste-Target\\dados.json";
        string json = File.ReadAllText(caminhoArquivo);

        List<FaturamentoDiario> faturamentoMensal = JsonSerializer.Deserialize<List<FaturamentoDiario>>(json);

        var diasValidos = faturamentoMensal.Where(v => v.valor > 0).ToList();

        var menorFaturamento = diasValidos.OrderBy(v => v.valor).First();
        Console.WriteLine($"Menor valor de faturamento: R$ {menorFaturamento.valor} no dia {menorFaturamento.dia}");

        var maiorFaturamento = diasValidos.OrderByDescending(v => v.valor).First();
        Console.WriteLine($"Maior valor de faturamento: R$ {maiorFaturamento.valor} no dia {maiorFaturamento.dia}");

        decimal mediaMensal = diasValidos.Average(v => v.valor);
        Console.WriteLine($"Média mensal de faturamento: R$ {mediaMensal:F2}");

        int diasAcimaDaMedia = diasValidos.Count(v => v.valor > mediaMensal);
        Console.WriteLine($"Número de dias com faturamento superior à média: {diasAcimaDaMedia}");

        // 4 - Cálculo percentual de faturamento por estado
        List<Estado> estados = new List<Estado>
        {
            new Estado { Nome = "SP", Faturamento = 67836.43m },
            new Estado { Nome = "RJ", Faturamento = 36678.66m },
            new Estado { Nome = "MG", Faturamento = 29229.88m },
            new Estado { Nome = "ES", Faturamento = 27165.48m },
            new Estado { Nome = "Outros", Faturamento = 19849.53m }
        };

        decimal totalFaturamento = estados.Sum(estado => estado.Faturamento);

        foreach (var estado in estados)
        {
            decimal percentual = (estado.Faturamento / totalFaturamento) * 100;
            Console.WriteLine($"Estado: {estado.Nome} - Faturamento: R$ {estado.Faturamento:F2} - Percentual: {percentual:F2}%");
        }



        // 5 - Programa que inverta os caracteres de uma string


        Console.WriteLine("Digite uma palavra: ");
        string palavra = Console.ReadLine();
        string palavraAoContrario = ReverterPalavra(palavra);
        Console.WriteLine($"{palavra} ao contrario = {palavraAoContrario}");

        string ReverterPalavra(string palavra)
        {
            palavra = palavra.Trim();
            char[] letras = palavra.ToCharArray();
            char[] palavraAoContrario = new char[letras.Length];

            int esquerda = 0;
            for (int i = palavra.Length - 1; i >= 0; i--)
            {
                palavraAoContrario[esquerda] = letras[i];
                esquerda++;
            }

            return new string(palavraAoContrario);
        }

    }


    static bool IsFibonacci(int numero)
    {
        return IsPerfectSquare(5 * numero * numero + 4) || IsPerfectSquare(5 * numero * numero - 4);
    }

    static bool IsPerfectSquare(int numero)
    {
        int raiz = (int)Math.Sqrt(numero);
        return raiz * raiz == numero;
    }
}

public class FaturamentoDiario
{
    public int dia { get; set; }
    public decimal valor { get; set; }
}

public class Estado
{
    public string Nome { get; set; }
    public decimal Faturamento { get; set; }
}
```
