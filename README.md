const cmdIcons = require('../../UI/icons/commandicons');
const { SlashCommandBuilder } = require('@discordjs/builders');
const { EmbedBuilder } = require('discord.js');
const lang = require('../../events/loadLanguage');

module.exports = {
  data: new SlashCommandBuilder()
    .setName('bot')
    .setDescription('Bot related commands.')
    .addSubcommand(subcommand =>
      subcommand
        .setName('ping')
        .setDescription(lang.pingDescription)
    ),

  async execute(interaction) {
    if (interaction.isCommand() && interaction.isCommand()) {
      const subcommand = interaction.options.getSubcommand();

      if (subcommand === 'ping') {
        const botLatency = Date.now() - interaction.createdTimestamp;
        const apiLatency = interaction.client.ws.ping;

        const embed = new EmbedBuilder()
          .setColor('#FFA500') // Orange color
          .setTitle(lang.pingTitle)
          .setDescription(`${lang.botLatency}: ${botLatency}ms\n${lang.apiLatency}: ${apiLatency}ms`)
          .setTimestamp();

        await interaction.reply({ embeds: [embed] });
      }
    } else {
      const embed = new EmbedBuilder()
        .setColor('#FFA500') // Orange color for the error too
        .setAuthor({
          name: "Alert!",
          iconURL: cmdIcons.dotIcon,
          url: "https://discord.gg/xQF9f9yUEM"
        })
        .setDescription('- This command can only be used through slash command!\n- Please use /bot')
        .setTimestamp();

      await interaction.reply({ embeds: [embed] });
    }
  },
};
