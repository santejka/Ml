from datetime import datetime, timedelta

# Предполагаем, что 'comb_data' уже загружен и представляет данные пользовательских сессий

# Преобразуем 'Session Start' и 'Session End' в datetime

comb_data['Session Start'] = pd.to_datetime(comb_data['Session Start'])

comb_data['Session End'] = pd.to_datetime(comb_data['Session End'])

# Вычисляем DAU

dau = comb_data.groupby(comb_data['Session Start'].dt.date)['User Id'].nunique().reset_index(name='DAU')

wau['Session Start'] = wau['Session Start'].dt.start_time.astype(str)

# Преобразуем 'Period' объекты в строки для MAU

mau['Session Start'] = mau['Session Start'].dt.start_time.astype(str)

# Рассчитываем общее время, проведенное в приложении, как разницу между 'Session End' и 'Session Start'

comb_data['Session Duration'] = (comb_data['Session End'] - comb_data['Session Start']).dt.total_seconds()

total_time_spent = comb_data['Session Duration'].sum()

# Распределение пользователей по странам, устройствам и каналам

country_dist = comb_data.groupby('Region')['User Id'].nunique().reset_index(name='Users by Country')

device_dist = comb_data.groupby('Device')['User Id'].nunique().reset_index(name='Users by Device')

channel_dist = comb_data.groupby('Channel')['User Id'].nunique().reset_index(name='Users by Channel')

# Теперь создаем дашборд с помощью Dash, включающий эти метрики

app.layout = html.Div([

html.H1('User Engagement Dashboard'),

dcc.Graph(

id='dau-graph',

figure=px.line(dau, x='Session Start', y='DAU', title='Daily Active Users (DAU)')

),

dcc.Graph(

id='wau-graph',

figure=px.bar(wau, x='Session Start', y='WAU', title

='Weekly Active Users (WAU)')

),

dcc.Graph(

id='mau-graph',

figure=px.bar(mau, x='Session Start', y='MAU', title='Monthly Active Users (MAU)')

),

html.H2(f'Total Time Spent in App: {total_time_spent} seconds'),

dcc.Graph(

id='country-dist',

figure=px.pie(country_dist, names='Region', values='Users by Country', title='User Distribution by Country')

),

dcc.Graph(

id='device-dist',

figure=px.pie(device_dist, names='Device', values='Users by Device', title='User Distribution by Device')

),

dcc.Graph(

id='channel-dist',

figure=px.pie(channel_dist, names='Channel', values='Users by Channel', title='User Distribution by Channel')

),

])

if __name__ == '__main__':

app.run_server(debug=True)


Как ИИ пошутил про евреев и политику и немного ML не судите строго⁠⁠

В одной маленькой стране произошли выборы, и в парламент впервые за много лет прошла партия евреев. На первом же заседании представитель этой партии поднял руку и говорит: "У меня есть два вопроса. Первый: почему в нашем парламенте так мало евреев? И второй: когда обед?"

Идея этого анекдота в том, чтобы подчеркнуть умение находить важное в любой ситуации и не терять при этом чувство юмора, а также затронуть классическую тему обсуждения важности представительства в политике и лёгкость, с которой переходят к повседневным вопросам.


как же он стелит)))
А также как предобработатать данные ML может комуто пригодиться)
Шаг 1: Загрузка данных

pythonCopy code

import pandas as pd # Загрузка данных visitation_data = pd.read_csv('/path/to/visitation.csv') orders_data = pd.read_csv('/path/to/orders.csv')
expenses_data = pd.read_csv('/path/to/expenses.csv')

visitation_data.drop_duplicates(inplace=True)
orders_data.drop_duplicates(inplace=True)
expenses_data.drop_duplicates(inplace=True)
# Обработка пропусков
visitation_data.fillna(value={'column_name': 'default_value'}, inplace=True)
orders_data.fillna(value={'Revenue': orders_data['Revenue'].mean()}, inplace=True)

# Приведение данных к формату

visitation_data['Session Start'] = pd.to_datetime(visitation_data['Session Start'])
visitation_data['Session End'] = pd.to_datetime(visitation_data['Session End'])
orders_data['Event Dt'] = pd.to_datetime(orders_data['Event Dt'])
expenses_data['dt'] = pd.to_datetime(expenses_data['dt'])

# Сохранение обработанных данных в формате CSV
visitation_data.to_csv('cleaned_visitation.csv', index=False)
orders_data.to_csv('cleaned_orders.csv', index=False)
expenses_data.to_csv('cleaned_expenses.csv', index=False)

SQL
import pandas as pd

# Чтение данных

data1 = pd.read_csv('data1.csv')

data2 = pd.read_csv('data2.csv')

data3 = pd.read_csv('data3.csv')

# Объединение DataFrame

combined_data = pd.concat([data1, data2, data3], ignore_index=True)


from sqlalchemy import create_engine

# Создание движка подключения к базе данных

engine = create_engine('sqlite:///mydatabase.db') # Пример для SQLite

# Загрузка объединенных данных в базу данных

combined_data.to_sql('combined_table', con=engine, if_exists='replace', index=False)
