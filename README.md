const { MessageEmbed } = require('discord.js');

module.exports = {
  name: 'ping',
  description: 'Check bot latency',
  execute(message) {
    // Check if the user has admin permission
    if (!message.member.permissions.has('ADMINISTRATOR')) {
      return message.reply("❌ You don't have permission to use this command.");
    }

    const botLatency = Date.now() - message.createdTimestamp;
    const apiLatency = message.client.ws.ping;

    const embed = new MessageEmbed()
      .setColor('#FFA500') // orange color
      .setTitle('🏓 Pong!')
      .setDescription(`Bot latency: ${botLatency}ms\nAPI latency: ${apiLatency}ms`)
      .setTimestamp();

    message.channel.send({ embeds: [embed] });
  }
};
