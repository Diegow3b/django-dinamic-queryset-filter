# django-dinamic-queryset-filter
Django - Dinamic Queryset Column Filter

Feel free to upgrade the function

```python
'''
	Os campos filtrados dessa versão são do tipo inteiro ou decimal
	Parametros:
	1 - Queryset: Qual queryset deseja filtrar
	2 - Coluna(String): Nome da coluna no qual deseja aplicar o filtro
	3 - Filtro(String): String com o valor no qual deseja filtrar			
        	- Menor ou Igual - "-<valor>" | Exemplo: "-250"
        	- Entre (Between) - "<valor>~<valor>" | Exemplo: "250~350"
        	- Maior ou igual - "+<valor>" | Exemplo: "+15"	
      
	The filtered filters of this version must be int or decimal
     	Parameters:
     	1 - Queryset
     	2 - Column(String): Name of the column you wish to apply the filter
     	3 - Filter(Stirng: String with the value you wish filter
        	- Less then Equals - "-<value>" | Exemple: "-250"
        	- Between - "<value>~<value> | Exemple: "250~350"
        	- Greater then Equals - "+<value>" | Exemple: "+15"
	'''
  	
    def filtro_dinamico(queryset, coluna, filtro):
        if '-' in filtro:
                filtro = filtro.replace('-','')
                queryset = queryset.filter(**{'{coluna}__lte'.format(coluna=coluna):int(filtro)})
        elif '~' in filtro:
            valores = filtro.split('~')
            if valores[0] and valores[1]:
                menor = valores[0]
                maior = valores[1]
                queryset = queryset.filter(**{'{coluna}__range'.format(coluna=coluna):(menor, maior)})
            else:
                raise ValidationError('Valor {filtro} da coluna {coluna} incorreto'.format(filtro=filtro, coluna=coluna))
        elif '+' in filtro:
            filtro = filtro.replace('+','')
            queryset = queryset.filter(**{'{coluna}__gte'.format(coluna=coluna):int(filtro)})
        
        return queryset
 ```
