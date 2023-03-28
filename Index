const Discord = require('discord.js');
const client = new Discord.Client();
const { joinVoiceChannel, createAudioPlayer, createAudioResource, AudioPlayerStatus } = require('@discordjs/voice');
const { TTSStream } = require('discord-tts');

const token = 'YOUR_BOT_TOKEN_HERE';
const prefix = '/';

client.on('ready', () => {
    console.log(`Logged in as ${client.user.tag}`);
});

client.on('message', async message => {
    if (message.author.bot) return;
    if (!message.content.startsWith(prefix)) return;

    const command = message.content.slice(prefix.length).trim().split(' ')[0];

    if (command === 'tts') {
        if (message.member?.voice.channel) {
            const connection = joinVoiceChannel({
                channelId: message.member.voice.channel.id,
                guildId: message.guild.id,
                adapterCreator: message.guild.voiceAdapterCreator
            });

            const player = createAudioPlayer();
            connection.subscribe(player);

            player.on(AudioPlayerStatus.Idle, () => {
                connection.destroy();
            });

            const ttsText = message.content.slice(prefix.length + command.length).trim();
            const ttsStream = new TTSStream(ttsText);

            const resource = createAudioResource(ttsStream, { inlineVolume: true });
            player.play(resource);
        } else {
            message.reply('You need to join a voice channel first!');
        }
    }
});

client.login(token);
