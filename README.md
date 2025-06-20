const { SlashCommandBuilder, EmbedBuilder } = require('@discordjs/builders');

module.exports = {
  data: new SlashCommandBuilder()
    .setName('ping')
    .setDescription('Check the bot latency'),

  async execute(interaction) {
    if (!interaction.isCommand()) return;

    if (!interaction.member.permissions.has('Administrator')) {
      return interaction.reply({ content: '❌ You do not have permission to use this command.', ephemeral: true });
    }

    const botLatency = Date.now() - interaction.createdTimestamp;
    const apiLatency = interaction.client.ws.ping;

    const embed = new EmbedBuilder()
      .setColor('#FFA500') // orange color
      .setTitle('🏓 Pong!')
      .setDescription(`Bot Latency: ${botLatency}ms\nAPI Latency: ${apiLatency}ms`)
      .setTimestamp();

    await interaction.reply({ embeds: [embed] });
  }
};
