# Telegram-bot
from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes


async def start(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    await update.message.reply_text("Salam! Bu menim ilk Telegram botumdur.")


async def main():
    TOKEN = "7309176475:AAF3JjnHwU_bREmX1iUxjIy4SH9FDP9eLWU"
    application = ApplicationBuilder().token(TOKEN).build()

    
application.add_handler(CommandHandler("start", start))
    await application.run_polling()
if __name__ == "__main__":
    import asyncio
    asyncio.run(main())
