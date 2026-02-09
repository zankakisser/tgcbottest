from telegram import Update
from telegram.ext import ApplicationBuilder, MessageHandler, ContextTypes, filters

TOKEN = "8486780607:AAFf1mJJMO0sJc1kfCz3vegY1zVWWO0JO9E"
CHANNEL = "@znkzcf"
TARGET_CHAT = -1003707680591

async def check_and_forward(update: Update, context: ContextTypes.DEFAULT_TYPE):
    user_id = update.effective_user.id

    try:
        member = await context.bot.get_chat_member(CHANNEL, user_id)

        if member.status != "left":
            await context.bot.forward_message(
                chat_id=TARGET_CHAT,
                from_chat_id=update.message.chat_id,
                message_id=update.message.message_id
            )
        else:
            await update.message.reply_text(
                "Подпишитесь на канал, чтобы отправлять сообщения."
            )

    except:
        await update.message.reply_text(
            "Ошибка проверки подписки."
        )

app = ApplicationBuilder().token(TOKEN).build()
app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, check_and_forward))
app.run_polling()
