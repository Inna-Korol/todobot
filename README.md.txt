import telebot
import random

token = "token"

bot = telebot.TeleBot(token)

HELP = """""
/help - íàïå÷àòàòü ñïðàâêó ïî ïðîãðàììå
/add - äîáàâèòü çàäà÷ó â ñïèñîê (íàçâàíèå çàäà÷è çàïðâøèâàåì ó ïîëüçîâàòåëÿ).
/show - íàïå÷àòàòü âñå äîáàâëåííûå çàäà÷è.
/random - äîáàâèòü ñëó÷àéíî çàäà÷ó íà äàòó Ñåãîäíÿ"""


RANDOM_TASKS = "Çàïèñàòüñÿ íà êóðñ â Íåòîëîãèþ", "Íàïèñàòü ïèñüìî", "Ïîìûòü ìàøèíó", "Ïîêîðìèòü êîøêó"

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
    text = "Çàäà÷à " + task + " Äîáàâëåíà íà äàòó " + date
    bot.send_message(message.chat.id, text)

@bot.message_handler(commands=["random"])
def random_add(message):
    date = "ñåãîäíÿ"
    task = random.choice(RANDOM_TASKS)
    add_todo(date, task)
    text = "Çàäà÷à " + task + " Äîáàâëåíà íà äàòó " + date
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
        text = "Çàäà÷ íà ýòó äàòó íåò"
    bot.send_message(message.chat.id, text)

bot.polling(none_stop=True)
