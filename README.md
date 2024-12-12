![image](https://github.com/user-attachments/assets/b6cc9fbe-ac53-44e2-a0ca-4831eca1bc4e)

# Abordagens de lematização: um olhar crítico sobre o processamento sintático na recuperação de dados linguísticos

Esse material notebook aborda o processamento sintático de textos em português brasileiro utilizando Python3 e abrange as etapas de importação, leitura, normalização, tokenização e lematização. O código foi desenvolvido como parte de um relatório para a disciplina de **Recuperação da Informação**, do curso de Ciência da Computação da Universidade Federal do Rio de Janeiro (UFRJ), ministrado pela professora Giseli Rabello em 2024 com apoio do monitor Henrique Fernandes Rodrigues. O dataset analisado pode ser encontrado em [D&G UFF](https://deg.uff.br/corpus-dg/).

**Mariana Gonçalves da Costa** [Programa de Pós-Graduação em Informática/UFRJ]

Last updated: 12 December 2024

## Corpus

O corpus do grupo Discurso & Gramática foi selecionado nessa demostração por estar disponível on-line e gratuitamente. Além disso, as características dos dados transcritos divergem dos textos normalmente tratados pelos algoritmos de lematização, podendo trazer resultados interessantes e apontar erros não perceptíveis em dados nativos da modalidade escrita.

OBS: As páginas de capa e fichamento foram removidas antes do processamento textual.

__Informações disponíveis no site do dataset__: 

Cada um dos informantes produziu cinco tipos distintos de textos orais e, a partir desses, cinco textos escritos, para assim garantir a comparabilidade entre os canais falado e escrito.
Tipos de textos: 
(1) narrativa de experiência pessoal; (2) narrativa recontada; (3) descrição de local; (4) relato de procedimento; e (5) relato de opinião.
Falantes selecionados por escolaridade:
(a) alunos de classe de alfabetização infantil; (b) alunos da 4ª série do Ensino Fundamental; (c) alunos da 8ª série do Ensino Fundamental; (d) alunos da 3ª série do Ensino Médio; e (e) alunos do último ano do Ensino Superior.
Processo de coleta e transcrição bem detalhado e disponível no site.


## Stopwords
Organizei as stopwords em um dicionário, agrupando-as pela classe de palavras.
Essa estrutura permite selecionar stopwords relevantes para a sua limpeza de dados de acordo com os objetivos do processamento de texto.

```Python
def select_stopwords(stopwords, word_classes, remove_list = None):
  """Cria uma lista de stopwords selecionadas a partir de um dicionário.

  Argumentos:
    stopwordsdict (dict): Dicionário contendo todas as stopwords por classe de palavras

    word_classes (list): A lista de classes de palavras a serem incluídas nas stopwords finais.
    As classes podem ser:
    artigos, pronomes, preposicoes, adverbios, conjuncoes, verbos, marcadores discursivos, números

    remove_list (list, opcional): Uma lista com palavras a serem removidas das stopwords

  Retorna:
    stopwords (list): Uma lista final de stopwords.
  """

  stopwords = [word for word_class in word_classes for word in stopwordsdict.get(word_class, [])]
  # Use dict.get() para evitar KeyError caso a classe não se encontre no dicionário

  # Remove palavras selecionadas da list de stopwords caso o argumento remove_list seja preenchido
  # com uma lista de remoção

  if remove_list:
      stopwords = [word for word in stopwords if word not in remove_list]

  return stopwords
```
### Exemplo de uso
```Python
stopwords = select_stopwords(stopwordsdict,
                            ["artigos", "pronomes", "preposicoes", "adverbios", "conjuncoes", "marcador", "numeros"],
                             remove_list = ["ser", "a", "o", "as","os"])
```
## Pontuações
Pontuações e demais símbolos encontrados nas transcrições = ", . / \ [ ] ( ) : ; ? ! � … - \x94"

## Abordagens de lematização

1.   **Lematização por aprendizado de máquina**: spaCy modelo pt_core_news_sm
   
Acurácia: 0.802
Realiza substituição por sinônimos.

2.   **Lematização por depêndencias universais**: Simplemma UD-PT-GSD

Acurácia: 0.825
Não realiza substituição por sinônimos.

3.   **Lematização por arquivos lexicográficos**: PortiLexicon-UD (Projeto POeTiSA)

Acurácia: 0.802
Não realiza substituição por sinônimos.
