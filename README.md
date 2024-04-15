# A escolha de um fornecedor

Problema: Escolher o fornecedor que melhor atenda o requisito de 4,80 ± 1,80 mm na dimensão principal da peça.

- Para isso foi disponibilizado 1000 dados de peças do Fabricante A e 1000 dados de peças do Fabricante B

### A grande dúvida que fica é, diante dessa amostra, somente analisando a porcentagem de erro, eu consigo tomar uma decisão assertiva?
Fica até o final desse documento para descobrir a resposta 

## Análise descritiva 
Box-Plot

Histograma

## Estimação de erro
```python
def calculate_error(df):
    df['error'] = 0
    df.loc[(df['dimensao_peca'] > (4.8 + 1.8)) | (df['dimensao_peca'] < (4.8 - 1.8)), 'error'] = 1
    return

calculate_error(df)

df.groupby('fornecedor')['error'].agg(['mean'])

Porcentagem de erro para Fornecedor A é: 71.9
Porcentagem de erro para Fornecedor B é: 34.7
```

Normalmente as empresas analisam até aqui. Porém, não podemos deixar de ressaltar que esses dados são uma **amostra** e não todas peças já fabricadas. 

Então fica o questionamento, será que podemos confiar nesse resultado?

## Teste de Hipótese

Para entender se de fato ambos os Fornecedores, possuem dados significativos, realizamos um teste estatistico.

