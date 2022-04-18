import telebot
import random

token = "5114504230:AAGrbS9RI03XEhhCGunkKoBh3uZRnIl8PUc"

bot = telebot.TeleBot(token)

HELP = """""
/help - ���������� ������� �� ���������
/add - �������� ������ � ������ (�������� ������ ����������� � ������������).
/show - ���������� ��� ����������� ������.
/random - �������� �������� ������ �� ���� �������"""


RANDOM_TASKS = "���������� �� ���� � ���������", "�������� ������", "������ ������", "��������� �����"

tasks  = {}


def add_todo(date, task):
    if date in tasks:
        tasks[date].append(task)
    else:
        tasks[date] = []
        tasks[date].append(task)


@bot.message_handler(commands=["help"])
def help(message):
    bot.send_message(message.chat.id, HELP)

@bot.message_handler(commands=["add"])
def add(message):
    command = message.text.split(maxsplit=2)
    date = command[1].lower()
    task = command[2]
    add_todo(date, task)
    text = "������ " + task + " ��������� �� ���� " + date
    bot.send_message(message.chat.id, text)

@bot.message_handler(commands=["random"])
def random_add(message):
    date = "�������"
    task = random.choice(RANDOM_TASKS)
    add_todo(date, task)
    text = "������ " + task + " ��������� �� ���� " + date
    bot.send_message(message.chat.id, text)

@bot.message_handler(commands=["show", "print"])
def show(message):
    command = message.text.split(maxsplit=1)
    date = command[1].lower()
    text = ""
    if date in tasks:
        text = date.upper() + "\n"
        for task in tasks[date]:
            text = text + "[] " + task + "\n"
    else:
        text = "����� �� ��� ���� ���"
    bot.send_message(message.chat.id, text)

bot.polling(none_stop=True)
