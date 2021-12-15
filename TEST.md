<% if (plugins.activity) { %>
  <%- await include(`partials/activity.ejs`) %>
<% } %>

<% if (plugins.wakatime) { %>
  <%- await include(`partials/wakatime.ejs`) %>
<% } %>
