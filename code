import logging
import requests
from aiogram import Bot, Dispatcher, executor, types

API_TOKEN = '7764035810:AAGCQtBbgDdcq2OfRc_bcekBvpJBbNJ1IzI'
WEATHER_API_KEY = '53adf836e50f19b6cab51be4dd465294'

logging.basicConfig(level=logging.INFO)

bot = Bot(token=API_TOKEN)
dp = Dispatcher(bot)


def get_weather(city: str) -> str:
    base_url = f'http://api.openweathermap.org/data/2.5/weather?q={city}&appid={WEATHER_API_KEY}&units=metric&lang=ru'
    response = requests.get(base_url)
    data = response.json()
    if data['cod'] != 200:
        return "Не удалось получить данные о погоде."

    weather_description = data['weather'][0]['description']
    temperature = int(data['main']['temp'])
    feels_like = int(data['main']['feels_like'])
    humidity = int(data['main']['humidity'])

    return (f"Погода в {city}:\n"
            f"Температура: {temperature}°C\n"
            f"Ощущается как: {feels_like}°C\n"
            f"Описание: {weather_description}\n"
            f"Влажность: {humidity}%")


@dp.message_handler(commands=['start'])
async def send_welcome(message: types.Message):
    await message.reply("Нажмите на вторую команду чтобы узнать погоду в Самаре")


@dp.message_handler(commands=['weather'])
async def send_weather(message: types.Message):
    await message.reply(get_weather('Samara'))


@dp.message_handler()
async def echo(message: types.Message):
    await message.answer("я не знаю такой команды")


if __name__ == '__main__':
    executor.start_polling(dp, skip_updates=True)
