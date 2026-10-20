#include <stdio.h>
#include <ctype.h>

int main() {
    int N, idade;
    char opiniao;
    
    // Contadores e acumuladores
    int qtd_otimos = 0;
    int qtd_bons = 0;
    int qtd_regulares = 0;
    
    int soma_idade_ruins = 0;
    int qtd_ruins = 0;
    
    int qtd_pessimos = 0;
    int maior_idade_pessimo = -1;
    
    int maior_idade_ruim = -1;
    int menor_idade_ruim = 999; // Para achar a diferença de idade se necessário
    
    printf("Digite o numero de espectadores (N): ");
    scanf("%d", &N);
    
    for (int i = 1; i <= N; i++) {
        printf("\nEspectador %d:\n", i);
        printf("Digite a idade: ");
        scanf("%d", &idade);
        
        // Limpa o buffer e lê o caractere
        do {
            printf("Digite o codigo da opiniao (A, B, C, D, E): ");
            scanf(" %c", &opiniao);
            opiniao = toupper(opiniao);
        } while (opiniao != 'A' && opiniao != 'B' && opiniao != 'C' && opiniao != 'D' && opiniao != 'E');
        
        // 1. Quantidade de Ótimos
        if (opiniao == 'A') {
            qtd_otimos++;
        }
        // Para a diferença percentual entre Bons e Regulares
        else if (opiniao == 'B') {
            qtd_bons++;
        }
        else if (opiniao == 'C') {
            qtd_regulares++;
        }
        // 3. Média de idade dos Ruins
        else if (opiniao == 'D') {
            soma_idade_ruins += idade;
            qtd_ruins++;
            
            // Para a última estatística (diferença de idade entre a maior e menor/outra idade que respondeu ruim)
            if (idade > maior_idade_ruim) {
                maior_idade_ruim = idade;
            }
            if (idade < menor_idade_ruim) {
                menor_idade_ruim = idade;
            }
        }
        // 4. Percentual de Péssimos e a maior idade com esta opinião
        else if (opiniao == 'E') {
            qtd_pessimos++;
            if (idade > maior_idade_pessimo) {
                maior_idade_pessimo = idade;
            }
        }
    }
    
    // --- CÁLCULOS FINAIS E EXIBIÇÃO ---
    
    // Estatística 1: Quantidade de Ótimos
    printf("\n--- ESTATISTICAS ---\n");
    printf("1. Quantidade de Otimos: %d\n", qtd_otimos);
    
    // Estatística 2: Diferença percentual entre Bons e Regulares
    // (Pode ser calculada em relação ao total de espectadores ou diretamente entre as quantidades)
    float diff_percentual = 0.0;
    if (N > 0) {
        float perc_bons = ((float)qtd_bons / N) * 100;
        float perc_regulares = ((float)qtd_regulares / N) * 100;
        diff_percentual = perc_bons - perc_regulares;
    }
    printf("2. Diferenca percentual entre Bons e Regulares: %.2f%%\n", diff_percentual);
    
    // Estatística 3: Média de idade dos Ruins
    float media_idade_ruins = 0.0;
    if (qtd_ruins > 0) {
        media_idade_ruins = (float)soma_idade_ruins / qtd_ruins;
    }
    printf("3. Media de idade dos Ruins: %.2f\n", media_idade_ruins);
    
    // Estatística 4: Percentual de Péssimos e a maior idade com esta opinião
    float perc_pessimos = 0.0;
    if (N > 0) {
        perc_pessimos = ((float)qtd_pessimos / N) * 100;
    }
    printf("4. Percentual de Pessimos: %.2f%% (Maior idade: %d)\n", perc_pessimos, maior_idade_pessimo != -1 ? maior_idade_pessimo : 0);
    
    // Estatística 5: Diferença de idade entre a maior idade (e outra referência) que respondeu Ruim
    int dif_idade_ruim = 0;
    if (qtd_ruins > 1) {
        dif_idade_ruim = maior_idade_ruim - menor_idade_ruim;
    }
    printf("5. Diferenca de idade (maior - menor) entre os que responderam Ruim: %d\n", dif_idade_ruim);

    return 0;
}
