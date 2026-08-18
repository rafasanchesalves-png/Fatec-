import pandas as pd

from google.colab import drive

drive.mount('/content/drive')

origem = '/content/drive/My Drive/Data/Comexstat/'

arquivo_1 = origem + 'EXP_2025.csv'
arquivo_2 = origem + 'EXP_2026.csv'
ncm = origem + 'NCM.csv'
pais = origem + 'PAIS.csv'
vias = origem + 'VIA.csv'
urf = origem + 'URF.csv'


exp25 = pd.read_csv(arquivo_1,low_memory=False,sep=';',encoding='utf-8')
exp26 = pd.read_csv(arquivo_2,low_memory=False,sep=';',encoding='utf-8')
expncm = pd.read_csv(ncm,low_memory=False,sep=';',encoding='latin-1')
exppais = pd.read_csv(pais,low_memory=False,sep=';',encoding='latin-1')
expvias = pd.read_csv(vias,low_memory=False,sep=';',encoding='utf-8')
expurf = pd.read_csv(urf,low_memory=False,sep=';',encoding='latin-1')

exp_final = pd.concat([exp25,exp26], ignore_index=True)

exp_final = exp_final.merge(exppais[['CO_PAIS','NO_PAIS']],on='CO_PAIS',how='left')
exp_final = exp_final.merge(expncm[['CO_NCM','NO_NCM_POR']],on='CO_NCM',how='left')

exp_final.head(5)

coluna = 'NO_PAIS_x'
cp = exp_final[coluna].value_counts().sort_values(ascending=False)
print (f'total de operações{coluna}:')
print(cp)

exp_final.to_csv(origem+'exp_final.csv',index=False)
