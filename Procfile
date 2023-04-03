web: gunicorn main:app











import pandas as pd

# Importar o arquivo CSV para um DataFrame do Pandas
df = pd.read_csv('base_de_dados.csv')

# Criar tabela "account"
account = df[['account_id', 'account_suitability']].drop_duplicates().reset_index(drop=True)
account.set_index('account_id', inplace=True)

# Criar tabela "asset"
asset = df[['asset_name', 'asset_cnpj', 'class_name']].drop_duplicates().reset_index(drop=True)
asset['asset_id'] = asset.index
asset.set_index('asset_id', inplace=True)

# Criar tabela "position"
position = df[['account_id', 'asset_name', 'position_value']]
position = pd.merge(position, account, on='account_id')
position = pd.merge(position, asset, on='asset_name')
position.drop(['account_suitability', 'asset_name'], axis=1, inplace=True)
position['position_id'] = position.index
position.set_index('position_id', inplace=True)

# Criar tabela intermediária "account_asset"
account_asset = pd.merge(position[['account_id', 'asset_id']], account, on='account_id')
account_asset = pd.merge(account_asset, asset, on='asset_id')
account_asset.drop(['account_suitability', 'asset_name'], axis=1, inplace=True)
account_asset.drop_duplicates(inplace=True)

# Imprimir as tabelas resultantes
print('Tabela "account":\n', account)
print('Tabela "asset":\n', asset)
print('Tabela "position":\n', position)
print('Tabela intermediária "account_asset":\n', account_asset)
