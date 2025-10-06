from twilio.rest import Client
from pytube import YouTube
import os

# إعدادات Twilio
account_sid = 'your_account_sid'
auth_token = 'your_auth_token'
client = Client(account_sid, auth_token)

# دالة لتنزيل الفيديو
def download_video(url):
    yt = YouTube(url)
    print(f"جاري تنزيل: {yt.title}")
    yt.streams.get_highest_resolution().download()
    print("تم التنزيل بنجاح!")
    return yt.title

# دالة لإرسال الفيديو إلى واتساب
def send_video(to_number, file_path):
    message = client.messages.create(
        body="فيديو جديد!",
        media_url='file:///' + file_path,
        from_='your_twilio_number',
        to=to_number
    )

# دالة لمعالجة الرسائل الواردة
def handle_incoming_message(message):
    url = message.body
    file_path = download_video(url)
    send_video(message.from_, file_path)

# هنا ستقوم بتشغيل البوت لاستقبال الرسائل والرد عليها
