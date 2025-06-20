const { Client, GatewayIntentBits, Events, EmbedBuilder } = require('discord.js');

const client = new Client({ intents: [GatewayIntentBits.Guilds] });

client.once(Events.ClientReady, () => {
  console.log(`Logged in as ${client.user.tag}`);
});

client.on(Events.InteractionCreate, async interaction => {
  if (!interaction.isChatInputCommand()) return;

  if (interaction.commandName === 'ping') {
    // Only allow administrators to use this command
    if (!interaction.member.permissions.has('Administrator')) {
      return interaction.reply({ content: '❌ You need admin permissions to use this command.', ephemeral: true });
    }

    const botLatency = Date.now() - interaction.createdTimestamp;
    const apiLatency = interaction.client.ws.ping;

    const embed = new EmbedBuilder()
      .setColor('#FFA500')
      .setTitle('🏓 Pong!')
      .setDescription(`Bot latency: ${botLatency}ms\nAPI latency: ${apiLatency}ms`)
      .setTimestamp();

    await interaction.reply({ embeds: [embed] });
  }
});

client.login('YOUR_BOT_TOKEN');
