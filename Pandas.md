# Pandas and Numpy

[Pandas](https://anaconda.org/anaconda/pandas)
[Numpy](https://anaconda.org/anaconda/numpy)

# Matplotlib

[Matplotlib](https://matplotlib.org/gallery/index.html)

## Leer archivo externo

```
df = pd.read_csv('data/winequality-red.csv', delimiter=';')
print(df.shape)
print(df.columns)
print(df.info())
print(df.describe())
print(df.head())
print(df.tail())
```

## Acceder alguna columna
```
df['chlorides']  # df.chlorides
```

## Los espacios siempre dan problemas, podemos eliminarlos de la siguiente forma
```
df2 = df.copy()
cols = df2.columns.tolist()
cols = [col.replace(' ', '_') for col in cols]
df2.columns = cols
df2.volatile_acidity
```

## Seleccionar varias columnas
```
df[['chlorides', 'volatile acidity']]
```

## Encontrar filas con `iloc` y `loc`
```
df.loc[0, 'fixed acidity']
df.loc[0:10, 'fixed acidity']
df.loc[10:15, ['chlorides', 'fixed acidity']]
```
```
df.iloc[0, 0]
df.iloc[0:10, 0]
df.iloc[10:15, [0, 4]]
```

## Crear una máscara
```
df['chlorides'] <= 0.08
```

## Usar una máscara para filtrar datos
```
df[df['chlorides'] <= 0.08]
df[(df['chlorides'] >= 0.04) & (df['chlorides'] < 0.08)]
df[(df['chlorides'] >= 0.04) & (df['chlorides'] < 0.08) & (df['pH'] > 3.5) & (df['pH'] < 4.00)]
```

## El método `query`
```
df.query('chlorides >= 0.04 and chlorides <= 0.08 and pH > 3.5 and pH < 4.00')
```

## El método `unique`
```
df['quality'].unique()
```

## Agrupar la información
```
df.groupby('quality')
```
### Explotar la información obtenida
```
groupby_obj = df.groupby('quality')
groupby_obj.mean()
groupby_obj.max()
groupby_obj.count()
df.groupby('quality').count()['fixed acidity']
df.groupby(['pH', 'quality']).count()['chlorides']
```

## Ordenar la información
```
df.sort_values('quality')
df.sort_values('quality', ascending=False)
df.sort_values(['quality', 'alcohol'], ascending=False)
```

## Crear y eliminar columnas
```
df['non_free_sulfur'] = df['total sulfur dioxide'] - df['free sulfur dioxide']

df.rename(columns={'total sulfur dioxide': 'total_sulfur_dioxide', 'free sulfur dioxide': 'free_sulfur_dioxide' }, inplace=True)

df.eval('non_free_sulfur2 = total_sulfur_dioxide - free_sulfur_dioxide', inplace=True)

df.head()

df.drop('non_free_sulfur2', axis=1)
df.columns  # Aún existe la columna!
df.drop('non_free_sulfur2', inplace=True, axis=1)
df.columns # Ya se fue!
```

## Datos nulos
```
df.fillna(-1, inplace=True)
df.dropna(inplace=True)
```

## Para graficar utilizamos la librería `matplotlib.pyplot`, así que tenemos que importarla
```
import matplotlib.pyplot as plt
%matplotlib inline
import matplotlib
matplotlib.style.use('ggplot')
```

### Algunos ejemplos de gráficas
```
df.plot(kind='hist')
df['quality'].plot(kind='hist')
df.hist(figsize=(10,10));
df.plot(kind='scatter', x='free_sulfur_dioxide', y='total_sulfur_dioxide')
df.plot(kind='box')
df[['fixed acidity', 'pH', 'alcohol']].plot(kind='box')
df[['fixed acidity', 'alcohol']].plot(kind='box')
```