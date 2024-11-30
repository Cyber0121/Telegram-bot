# Telegram-bot
import telebot

# Bot tokeninizi daxil edin
BOT_TOKEN = "7309176475:AAF3JjnHwU_bREmX1iUxjIy4SH9FDP9eLWU"
bot = telebot.TeleBot(BOT_TOKEN)

# /ban komandası üçün funksiya
@bot.message_handler(commands=['ban'])
def ban_user(message):
    # Əgər istifadəçi qrupda deyilsə, dayandır
    if not message.chat.type in ['group', 'supergroup']:
        bot.reply_to(message, "Bu komanda yalnız qruplarda istifadə edilə bilər.")
        return

    # Bot admin deyilsə, xəbərdar et
    chat_admins = bot.get_chat_administrators(message.chat.id)
    if not any(admin.user.id == bot.get_me().id for admin in chat_admins):
        bot.reply_to(message, "Məni admin etməlisiniz ki, birini banlaya bilim!")
        return

    # Kim banlanacaq? Cavab verilmiş mesaja bax
    if not message.reply_to_message:
        bot.reply_to(message, "Bir istifadəçini banlamaq üçün həmin istifadəçinin mesajına cavab verin.")
        return

    user_to_ban = message.reply_to_message.from_user.id
    try:
        bot.ban_chat_member(message.chat.id, user_to_ban)
        bot.reply_to(message, "İstifadəçi uğurla banlandı.")
    except Exception as e:
        bot.reply_to(message, f"Ban etmək mümkün olmadı: {e}")

# Botu işə sal
bot.polling()
