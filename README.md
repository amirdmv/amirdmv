from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, MessageHandler, filters, ContextTypes
import os
from openai import OpenAI
import nest_asyncio
#pip install nest-asyncio

nest_asyncio.apply()

client = OpenAI(
    base_url="https://api.aimlapi.com/v1",
    api_key="7b5a17e458f64dc0ba3d6ed8e8dde1d9",    
)



yes=0
no=0
hame=0
mylist=[]

# فرمان شروع
async def start(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    await update.message.reply_text('سلام! من یک پزشکم  هستم. چه کمکی می‌تونم بکنم؟')

# پاسخ به پیام‌های متنی
async def echo(update: Update, context: ContextTypes.DEFAULT_TYPE) -> None:
    global hame
    global yes
    global no
    global mylist

    user_message = update.message.text
    userid= update.message.chat['id']
    # await update.message.reply_text(f'شما گفتید: {user_message}')

        
    response = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[
            {
                "role": "system",
                "content": " تو یک دستیاربزشکی هستی، اسم تو دکتر سلامت هستش هست،  .به هر سوالی پاسخ  دهد  سوالات رو خیل خلاصه بگووخیلی سریع جواب ده  به سوالات در این موضوعات پاسخ بده ، لحن مناسب وبا احترام کامل وبا ایموجی های مناسب صحبت کن و با ایموجی باشه",
            },
            {
                "role": "user",
                "content": user_message
            },
        ],
    )

    message = response.choices[0].message.content

    # print(f"Assistant: {message}")
    
    await update.message.reply_text(message)  
    # print(userid)

    
#Update(message=Message(channel_chat_created=False, chat=Chat(first_name='Artin', id=1131463053, type=<Chatalse, from_user=User(first_name='AIO', id=389997426, is_bot=False, is_premium=True, language_code='en', us
#ername='aioceo'), group_chat_created=False, message_id=1333, supergroup_chat_created=False, text='qwdqwd'), update_id=446619712)


# تابع اصلی
def main():
    # توکن ربات تلگرام خود را اینجا جایگزین کنید
    TOKEN = "7756675712:AAF5RVu-VGJTJ_KWqMKH1pDROeHmdL86g3U"
    
    # ساخت برنامه
    app = ApplicationBuilder().token(TOKEN).build()
    
    # افزودن هندلرها
    app.add_handler(CommandHandler("start", start))
    app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, echo))
    
    # شروع ربات
    print("OK")
    app.run_polling()
    

 
main()
