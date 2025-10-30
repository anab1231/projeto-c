#include <stdio.h>


void bubbleSort(int v[], int n);
void insertionSort(int v[], int n);
void selectionSort(int v[], int n);
void mergeSort(int v[], int l, int r);
void merge(int v[], int l, int m, int r);
void quickSort(int v[], int low, int high);
int partition(int v[], int low, int high);
void alterarValorNoArquivo();


int main() {
    int n, i, opcao;
    int numeros[100];

    printf("Quantos numeros voce quer digitar? ");
    scanf("%d", &n);

    for(i=0;i<n;i++){
        printf("Digite o numero %d: ", i+1);
        scanf("%d", &numeros[i]);
    }

    printf("\nEscolha o tipo de ordenacao:\n");
    printf("1 - Bubble Sort\n2 - Insertion Sort\n3 - Selection Sort\n4 - Merge Sort\n5 - Quick Sort\nOpcao: ");
    scanf("%d", &opcao);

    switch(opcao){
        case 1: bubbleSort(numeros,n); break;
        case 2: insertionSort(numeros,n); break;
        case 3: selectionSort(numeros,n); break;
        case 4: mergeSort(numeros,0,n-1); break;
        case 5: quickSort(numeros,0,n-1); break;
        default: 
            printf("Opcao invalida!\n"); 
            return 0;
    }

    FILE *f = fopen("ordenado.txt","w");
    if(f==NULL){
        printf("Erro ao criar arquivo!\n");
        return 0;
    }
    for(i=0;i<n;i++)
        fprintf(f,"%d ",numeros[i]);
    fclose(f);

    printf("\nNumeros ordenados gravados em 'ordenado.txt'!\n");

    // Perguntar se o usuário quer alterar algum valor
    char resposta;
    printf("\nDeseja alterar algum valor no arquivo? (s/n): ");
    scanf(" %c", &resposta);
    if(resposta=='s' || resposta=='S'){
        alterarValorNoArquivo();
    }

    return 0;
}


void alterarValorNoArquivo(){
    int numeros[100], n=0, pos, novoValor;
    FILE *f = fopen("ordenado.txt","r");
    if(!f){ printf("Erro ao abrir 'ordenado.txt'!\n"); return; }

    while(fscanf(f,"%d",&numeros[n])!=EOF) n++;
    fclose(f);

    printf("\nNumeros atuais:\n");
    for(int i=0;i<n;i++) printf("[%d] %d\n", i, numeros[i]);

    printf("\nDigite a posicao que deseja alterar (0 a %d): ", n-1);
    scanf("%d",&pos);
    if(pos<0 || pos>=n){ printf("Posicao invalida!\n"); return; }

    printf("Digite o novo valor: ");
    scanf("%d",&novoValor);
    numeros[pos]=novoValor;

   
    int opcao;
    printf("\nEscolha o tipo de ordenacao apos alterar:\n");
    printf("1 - Bubble Sort\n2 - Insertion Sort\n3 - Selection Sort\n4 - Merge Sort\n5 - Quick Sort\nOpcao: ");
    scanf("%d",&opcao);

    switch(opcao){
        case 1: bubbleSort(numeros,n); break;
        case 2: insertionSort(numeros,n); break;
        case 3: selectionSort(numeros,n); break;
        case 4: mergeSort(numeros,0,n-1); break;
        case 5: quickSort(numeros,0,n-1); break;
        default: printf("Opcao invalida! Arquivo nao sera reordenado.\n"); break;
    }

    f=fopen("ordenado.txt","w");
    for(int i=0;i<n;i++) fprintf(f,"%d ",numeros[i]);
    fclose(f);

    printf("\nArquivo 'ordenado.txt' atualizado com sucesso!\n");
}


void bubbleSort(int v[], int n){ int i,j,aux; for(i=0;i<n-1;i++) for(j=0;j<n-i-1;j++) if(v[j]>v[j+1]){ aux=v[j]; v[j]=v[j+1]; v[j+1]=aux; } }
void insertionSort(int v[], int n){ int i,j,key; for(i=1;i<n;i++){ key=v[i]; j=i-1; while(j>=0 && v[j]>key){ v[j+1]=v[j]; j--; } v[j+1]=key; } }
void selectionSort(int v[], int n){ int i,j,min_idx,aux; for(i=0;i<n-1;i++){ min_idx=i; for(j=i+1;j<n;j++) if(v[j]<v[min_idx]) min_idx=j; aux=v[i]; v[i]=v[min_idx]; v[min_idx]=aux; } }
void merge(int v[], int l, int m, int r){ int i,j,k; int n1=m-l+1,n2=r-m,L[n1],R[n2]; for(i=0;i<n1;i++) L[i]=v[l+i]; for(j=0;j<n2;j++) R[j]=v[m+1+j]; i=j=0;k=l; while(i<n1 && j<n2){ if(L[i]<=R[j]) v[k++]=L[i++]; else v[k++]=R[j++]; } while(i<n1) v[k++]=L[i++]; while(j<n2) v[k++]=R[j++]; }
void mergeSort(int v[], int l, int r){ if(l<r){ int m=l+(r-l)/2; mergeSort(v,l,m); mergeSort(v,m+1,r); merge(v,l,m,r); } }
int partition(int v[], int low, int high){ int pivot=v[high],i=low-1,aux; for(int j=low;j<high;j++){ if(v[j]<=pivot){ i++; aux=v[i]; v[i]=v[j]; v[j]=aux; } } aux=v[i+1]; v[i+1]=v[high]; v[high]=aux; return i+1; }
void quickSort(int v[], int low, int high){ if(low<high){ int pi=partition(v,low,high); quickSort(v,low,pi-1); quickSort(v,pi+1,high); } }
