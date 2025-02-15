# Prominent-rename
Renaming bot
import os
from pyrogram import Client, filters
from pyrogram.types import Message, InlineKeyboardMarkup, InlineKeyboardButton

# Bot Configuration
API_ID = "23763455"  # Replace with your API ID
API_HASH = "0838332572e88c0eba64fbadebb71e7f"  # Replace with your API Hash
BOT_TOKEN = "7473330997:AAHyGT_M-Q7Og7TgQ0b-tey4UEXkw29eOsI"  # Replace with your Bot Token

# Initialize the bot
bot = Client("Prominent_rename", api_id=API_ID, api_hash=API_HASH, bot_token=BOT_TOKEN)

# Dictionary to track users' uploaded files
user_files = {}

@bot.on_message(filters.private & filters.document)
async def document_handler(client, message: Message):
    """Handles file uploads."""
    file = message.document
    user_files[message.chat.id] = file.file_id

    await message.reply_text(
        f"📂 **Received:** `{file.file_name}`\n\n"
        "🔄 **Send the new name (with extension)**:",
        reply_markup=InlineKeyboardMarkup([
            [InlineKeyboardButton("Cancel", callback_data="cancel")]
        ])
    )

@bot.on_message(filters.private & filters.text)
async def rename_handler(client, message: Message):
    """Handles file renaming requests."""
    user_id = message.chat.id
    new_name = message.text.strip()

    if user_id not in user_files:
        await message.reply_text("⚠️ No file found! Please upload a file first.")
        return

    file_id = user_files.pop(user_id)
    sent_msg = await message.reply_text("⏳ Renaming...")

    # Download the file
    file_path = await bot.download_media(file_id)
    new_path = os.path.join(os.path.dirname(file_path), new_name)

    # Rename the file
    os.rename(file_path, new_path)

    # Send the renamed file
    await bot.send_document(user_id, new_path, caption=f"✅ Renamed to `{new_name}`")

    # Cleanup
    os.remove(new_path)
    await sent_msg.delete()

@bot.on_callback_query(filters.regex("cancel"))
async def cancel_rename(client, callback_query):
    """Handles cancel button."""
    await callback_query.message.edit_text("❌ Operation cancelled.")

# Start the bot
print("Bot is running...")
bot.run()import os
from pyrogram import Client, filters
from pyrogram.types import Message, InlineKeyboardMarkup, InlineKeyboardButton

# Bot Configuration
API_ID = "23763455"  # Replace with your API ID
API_HASH = "0838332572e88c0eba64fbadebb71e7f"  # Replace with your API Hash
BOT_TOKEN = "7473330997:AAHyGT_M-Q7Og7TgQ0b-tey4UEXkw29eOsI"  # Replace with your Bot Token

# Initialize the bot
bot = Client("Prominent_rename", api_id=API_ID, api_hash=API_HASH, bot_token=BOT_TOKEN)

# Dictionary to track users' uploaded files
user_files = {}

@bot.on_message(filters.private & filters.document)
async def document_handler(client, message: Message):
    """Handles file uploads."""
    file = message.document
    user_files[message.chat.id] = file.file_id

    await message.reply_text(
        f"📂 **Received:** `{file.file_name}`\n\n"
        "🔄 **Send the new name (with extension)**:",
        reply_markup=InlineKeyboardMarkup([
            [InlineKeyboardButton("Cancel", callback_data="cancel")]
        ])
    )

@bot.on_message(filters.private & filters.text)
async def rename_handler(client, message: Message):
    """Handles file renaming requests."""
    user_id = message.chat.id
    new_name = message.text.strip()

    if user_id not in user_files:
        await message.reply_text("⚠️ No file found! Please upload a file first.")
        return

    file_id = user_files.pop(user_id)
    sent_msg = await message.reply_text("⏳ Renaming...")

    # Download the file
    file_path = await bot.download_media(file_id)
    new_path = os.path.join(os.path.dirname(file_path), new_name)

    # Rename the file
    os.rename(file_path, new_path)

    # Send the renamed file
    await bot.send_document(user_id, new_path, caption=f"✅ Renamed to `{new_name}`")

    # Cleanup
    os.remove(new_path)
    await sent_msg.delete()

@bot.on_callback_query(filters.regex("cancel"))
async def cancel_rename(client, callback_query):
    """Handles cancel button."""
    await callback_query.message.edit_text("❌ Operation cancelled.")

# Start the bot
print("Bot is running...")
bot.run(/start)
