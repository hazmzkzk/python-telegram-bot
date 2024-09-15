import requests
from telegram import Update
from telegram.ext import Updater, CommandHandler, CallbackContext

# توكن بوت تلجرام
TELEGRAM_BOT_TOKEN = 7284795091:AAHgbrJcU05OolS7PzTzcOG5tUtJKOzNKoU'

# API Key الخاص بحسابك في الموقع
API_KEY = 7bfd4719e3bb9cc568ac96e9f406debb'

# رابط API للشراء (استبدل هذا بالرابط الصحيح)
API_URL = https://smmsuply.com/api/v2'

# دالة لمعالجة الأمر /buy
def buy(update: Update, context: CallbackContext) -> None:
    # مثال على كيفية الحصول على بيانات من المستخدم
    amount = context.args[0] if context.args else '1'  # القيمة الافتراضية 1

    # معلومات العملية
    data = {
        'currency': 'BTC',  # العملة التي تريد شرائها
        'amount': amount,    # المبلغ الذي يريده المستخدم
    }

    # إعداد الرأس مع API Key
    headers = {
        'Authorization': f'Bearer {API_KEY}',
    }

    # إجراء الطلب
    response = requests.post(API_URL, headers=headers, json=data)

    # التحقق من الاستجابة
    if response.status_code == 200:
        update.message.reply_text('تمت عملية الشراء بنجاح!')
    else:
        update.message.reply_text('حدث خطأ: ' + response.json().get('message', ''))

def main() -> None:
    # إعداد البوت
    updater = Updater(TELEGRAM_BOT_TOKEN)

    # إضافة معالج الأمر
    dispatcher = updater.dispatcher
    dispatcher.add_handler(CommandHandler('buy', buy))

    # بدء البوت
    updater.start_polling()
    updater.idle()

if __name__ == '__main__':
    main()