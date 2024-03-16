import discord
from discord.ext import commands
from monetkaa import monetka
from tokenn import TOKEN

ZAPRET_WORDS=['fack', 'faker']

intents = discord.Intents.default()
intents.message_content = True

bot = commands.Bot(command_prefix='!', intents=intents)

@bot.event
async def on_ready():
    print(f'Logged in as {bot.user}') #(ID: {bot.user.id})
    #await message.channel.send(f"Бот {bot.user} готов к работе")

@bot.command()
async def hello(ctx):
    await ctx.send(f'Привет! Я бот {bot.user}!')

@bot.command()
async def heh(ctx, count_heh = 5):
    await ctx.send("he" * count_heh)


@bot.event
async def on_message(message):
    await bot.process_commands(message)

    emb = discord.Embed(title='Вы:', description=f'{message.author.name}#{message.author.discriminator}', color=0x6cf4f0)
    for content in message.content.split():
        for zapert_word in ZAPRET_WORDS:
            if content.lower()  == zapert_word:
                await message.delete()
                await message.channel.send(f"{message.author.mention} такие слова запрещены!")

    if message.content.startswith('Подбросить монетку'):
        await message.channel.send(monetka())
    elif message.content.startswith('Кто я'):
        await message.channel.send(embed=emb)
        

bot.run(TOKEN)
