#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#ifdef _WIN32

#define CLEAR "cls"

#else

#define CLEAR "clear"

#endif
/*variaveis globais: */
char tabuleiro[8][64];
int nivel=0, final;

void printar_tela(int vet[]){

    printf("_______   _______  _______  _______  _______  _______  _______  _______\n");
    printf("|     |   | %2i  |  | %2i  |  | %2i  |  | %2i  |  | %2i  |  | %2i  |  |     |\n", vet[12], vet[11], vet[10], vet[9],vet[8], vet[7] );
    printf("|     |   |_____|  |_____|  |_____|  |_____|  |_____|  |_____|  |     |\n");
    printf("| %2i  |                                                         | %2i  |\n", vet[13], vet[6]);
    printf("|     |   _______  _______  _______  _______  _______  _______  |     |\n");
    printf("|     |   | %2i  |  | %2i  |  | %2i  |  | %2i  |  | %2i  |  | %2i  |  |     |\n", vet[0], vet[1], vet[2], vet[3], vet[4], vet[5]);
    printf("|_____|   |_____|  |_____|  |_____|  |_____|  |_____|  |_____|  |_____|\n");
    printf("             A        B        C        D        E        F           \n");
}


typedef struct no{
	int vet[14];
	struct no * filho[6];
}t_no;

t_no * criarNo(int tabuleiro[]){
   t_no * n =  (t_no *)malloc(sizeof(t_no));
   int i;
	for(i=0; i<6; i++){
		n->filho[i] = NULL;
	}
   for(i = 0; i<=13;i++){
   	n->vet[i] = tabuleiro[i];
   }

   return n;
}

int Prioridade(int vet[], int j1, int ultima, int repetiu){
	int pc=0, usuario=0, i, pontuacao=0;

	if(repetiu == 1){
		pontuacao+=30;
	}
	for(i=7;i<13;i++){
		pc+=vet[i];
	}
	for(i=0;i<6;i++){
		usuario+=vet[i];
	}


	if(vet[13]-25 < vet[6]-25 ){
		pontuacao -= 3;
	}
	else{
		if(vet[13]-25> vet[6]-25){
			pontuacao += 3;
		}
	}

	if(vet[13]>vet[6] && pc>usuario ){
		pontuacao+= 5;
	}
	else{
		if(vet[13]> vet[6] && pc<usuario){
			pontuacao +=1;
		}
		else{
			pontuacao -= 3;
		}
	}
	return pontuacao;
}

void roubarpecas(int ultima, int *j1,int vet[], int ind){

	if(*j1 == 0 && ultima != 13 && ultima <13 && ultima >=7){
		vet[13] = vet[13] + vet[ultima] + vet[ind];
		vet[ultima] = 0;
		vet[ind] = 0;			
	}
	else{
		if(*j1 == 1 && ultima != 6 && ultima <6 && ultima >=0){
			vet[6] = vet[6] + vet[ultima] + vet[ind];
			vet[ultima] = 0;
			vet[ind] = 0;
		}
	}		
}
int mexerpecas2(int vet[], char escolha, int i, int *j1){
	int num, ultima, ind;

	num = vet[i];
	vet[i] = 0;

	i++;
	while(num != 0){
		if(*j1 == 1){
			if(i == 13){
				i=0;
			}
		}
		else{
			if(i == 6){
				i = 7;
			}
			if(i==14){
				i=0;
			}	
		}
		vet[i] += 1;
		num--;
		i++;
	}
	ultima = i - 1;
	ind = 12 - ultima;


	

	if(vet[ind] > 0 && vet[ultima] == 1){
		roubarpecas(ultima, j1, vet, ind);
	}
		if(*j1 == 1 && ultima != 6){
		*j1=0;
	}
	else{
		if(*j1== 0 && ultima != 13){
			*j1 = 1;
		}	
	}
	return ultima;	
}	
int mexerpecas(int vet[], char escolha, int j1){
	int  i=0, ultima;	

	if(escolha == 'A' || escolha == 'a' ){
		if( j1 == 1){
			i = 0;
		}
		else{
			i = 7;
		}
		if(vet[i] != 0){
			ultima = mexerpecas2(vet, escolha, i, &j1);
		}
		else{
			ultima =-1;
		}
	}
	else{
		if(escolha == 'B' || escolha == 'b'){
			if( j1 == 1){
				i = 1;
			}
			else{
				i = 8;
			}
			if(vet[i] != 0){
				ultima = mexerpecas2(vet, escolha, i, &j1);
			}
			else{
				ultima =-1;
			}
		}
		else{
			if(escolha == 'C' || escolha == 'c'){
				if(j1 == 1){
					i = 2;
				}
				else{
					i = 9;
				}
				if(vet[i] != 0){
					ultima = mexerpecas2(vet, escolha, i, &j1);
				}
				else{
					ultima =-1;
				}
			}
			else{
				if(escolha == 'D' || escolha == 'd'){
					if(j1 == 1){
						i = 3;
					}
					else{
						i = 10;
					}
					if(vet[i] != 0){
						ultima = mexerpecas2(vet, escolha, i, &j1);	

					}		
					else{
						ultima =-1;
					}
				}
				else{
					if(escolha == 'E' || escolha == 'e'){
						if(j1 == 1){
							i = 4;
						}
						else{
							i = 11;
						}
						if(vet[i] != 0){
							ultima = mexerpecas2(vet, escolha, i, &j1);	
						}
						else{
							ultima =-1;
						}
				
					}
					else{
						if(escolha == 'F' || escolha == 'f'){
							if(j1 == 1){
								i = 5;	
							}
							else{
								i = 12;
							}
							if(vet[i] != 0){
								ultima = mexerpecas2(vet, escolha, i, &j1);	
							}
							else{
								ultima =-1;
							}
						}	
					}
				}
			}
		}
	}
	return ultima;
}	
int MinMax(t_no * n,int profundidade , int vez_jogador, int ultima, int repetiu){
	int alfa,inicio_jog=0, i =0, final, aux;
	char escolha;

	if(n == NULL || profundidade == nivel){
		return Prioridade(n->vet, vez_jogador, ultima, repetiu);
		
	}
	else{
		if(vez_jogador==0){
			alfa = -10000;
			inicio_jog = 7;
			for(i=0;i<6;i++){
				if(n->vet[inicio_jog] != 0){
					n->filho[i] = criarNo(n->vet);
					if(inicio_jog == 7){
						escolha = 'a';
					}
					else{
						if(inicio_jog == 8){
							escolha = 'b';
						}
						else{
							if(inicio_jog==9){
								escolha = 'c';
							}
							else{
								if(inicio_jog==10){
									escolha = 'd';
								}
								else{
									if(inicio_jog == 11){
										escolha = 'e';
									}
									else{
										escolha = 'f';
									}
								}
							}
						}
					}
					ultima = mexerpecas(n->filho[i]->vet, escolha, vez_jogador);
					if(ultima == 13){
						repetiu =1;
					}
					aux = MinMax(n->filho[i], profundidade+1,1, ultima, repetiu);
					if (aux > alfa) {
						alfa = aux;
						final = inicio_jog;
					}		
				}
				inicio_jog++;

			}
			return final;
		}
		else{
			alfa = 10000;
			inicio_jog = 0;
			for(i=0;i<6;i++){
				if(n->vet[inicio_jog] != 0){
					n->filho[i] = criarNo(n->vet);
					if(inicio_jog == 0){
						escolha = 'a';
					}
					else{
						if(inicio_jog == 1){
							escolha = 'b';
						}
						else{
							if(inicio_jog==2){
								escolha = 'c';
							}
							else{
								if(inicio_jog==3){
									escolha = 'd';
								}
								else{
									if(inicio_jog ==4 ){
										escolha = 'e';
									}
									else{
										escolha = 'f';
									}
								}
							}
						}
					}
					ultima = mexerpecas(n->filho[i]->vet, escolha, vez_jogador);
					aux =  MinMax(n->filho[i], profundidade+1, 0 , ultima, repetiu);
					if (alfa>aux) {
						alfa = aux;
						final = inicio_jog;
					}	
				}	
				inicio_jog++;
			}
			return -final;
		}
	return final;
	}
	return 0;
}
int acabou(int vet[]){
	int i;
	if(vet[0] == 0 && vet[1] == 0 && vet[2] == 0 && vet[3] == 0 && vet[4] == 0 && vet[5] == 0 ){
		for(i=7;i<13;i++){
			vet[13] += vet[i];
			vet[i]=0;
		}
		return 1;
	}
	else{
		if((vet[7]== 0 && vet[8] == 0 && vet[9] == 0 && vet[10] == 0 && vet[11] == 0 && vet[12] == 0)){
			for(i=0;i<6;i++){
				vet[6] += vet[i];
				vet[i]=0;
			}

			return 1;
		}
	}

	return 0;
}
int main(){
	int fim= 0, vet[14], j1=1, profundidade, jogarnovamente, casa, i, decisao;
	char escolha;
	t_no * n; 

	for(i=0; i<13;i++){
		vet[i]=4;
		
	}
	vet[6] = 0;
	vet[13] = 0;

	printar_tela(vet);
    printf("Digite o nivel desejado:\n1)Facil\n2)Medio\n3)Dificil\n");
    scanf("%d", &decisao);
    if(decisao ==1){
    	nivel =3;
    }
    else{
    	if(decisao == 2){
    		nivel = 5;
    	}
    	else{
    		nivel = 7;
    	}
    }
    n = criarNo(vet);

    while(fim == 0){
    	while(j1==1){
    		system(CLEAR);
    		n = criarNo(n->vet);
    		printar_tela(n->vet);
    		printf("Escolha a posicao:\n");
    		scanf(" %c", &escolha);
			while(getchar() != '\n');

    		jogarnovamente = mexerpecas(n->vet, escolha, j1);

    		system(CLEAR);
    		printar_tela(n->vet);
    		fim = acabou(n->vet);
			if(fim == 0){  	  
	    		if(jogarnovamente==6 ){
	    			j1 = 1;
	    			printf("Jogue novamente:\n");
	    		}
	    		else{
	    			if(jogarnovamente == -1){
	    				j1=1;
	    				printf("\nCasa vazia!! Jogue novamente.\n");
	    			}
	    			else{
	    				j1=0;
	    			}
	    		}
			}
			else{
				j1=-1;
				printf("\nAcabou o jogo!!\n");
    		}


    		printf("Aperte <enter> para continuar!");
    		getchar();
    	}
 		while(j1==0){
 			system(CLEAR);
    		profundidade =0;
    		final=0;
   			printf("Jogada do computador...\n");
   			n = criarNo(n->vet);

   			casa = MinMax(n, profundidade , 0, 0, 0);
   			if(casa == 7){
		  		escolha = 'a';
		  	}
		  	else {
		  		if(casa == 8){
		  			escolha = 'b';
		  		}
		  		else{
		  			if(casa == 9){
		  				escolha = 'c';
		  			}
		  			else{
		  				if(casa == 10){
		  					escolha = 'd';
		  				}
		  				else{
		  					if(casa == 11){
		  						escolha = 'e';
		  					}
		  					else{
		  						if(casa == 12){
		  							escolha = 'f';
		  						}
		  					}
		  				}
		  			}
		  		}
		  	}

		  	
		  	jogarnovamente = mexerpecas(n->vet, escolha, j1);
		  	
		  	for(i=0;i<13;i++){
		  		vet[i] = n->vet[i];
		  	}
		  	printar_tela(vet);

    		fim = acabou(n->vet);
			if(fim == 0){  

				if(jogarnovamente == 13){
			  		j1 = 0;
			  		printf("Jogada do computador novamente...\n");
			  	}else{
	   				j1 = 1;
				}
			}
			else{
				j1=1;
				printf("\nAcabou o jogo!!\n");
			}	


    		printf("Aperte <enter> para continuar!");
    		getchar();
    	}


    }

		system(CLEAR);
		printf("FIM DE JOGO!!!");
		if(n->vet[13]> n->vet[6]){
			printf("\nO computador ganhou!!\n");
		}
		else{
			printf("\nParabens, voce ganhou!!\n");
		}
		printar_tela(n->vet);
		printf("\nAperte <enter> para sair!\n");
		getchar();
	return 0;
}
