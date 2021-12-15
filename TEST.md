<% if (plugins.activity) { %>
  <%- await include(`partials/activity.ejs`) %>
<% } %>

<% if (plugins.wakatime) { %>
  <%- await include(`https://raw.githubusercontent.com/ZTzTopia/ZTzTopia/master/partials/wakatime.ejs`) %>
<% } %>
