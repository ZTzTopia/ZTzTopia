<%- await include(`partials/activity.ejs`) %>

<% if (plugins.wakatime) { %>
**⏰ WakaTime <%= plugins.wakatime?.days ? `(over last ${{7:"week", 30:"month", 180:"6 months", 365:"year"}[plugins.wakatime.days]})` : "" %>**
  <% if (plugins.wakatime.error) { %>
    <%= plugins.wakatime.error.message %>
  <% } else { %>
    <% // left %>
    <% if (plugins.wakatime.sections.includes("time")) { %>
  ~<%= f(Math.ceil(plugins.wakatime.time.total)) %> coding hour<%= s(plugins.wakatime.time.total) %> recorded
    <% } %>
    <% if ((plugins.wakatime.sections.includes("projects"))&&(plugins.wakatime.projects?.length)) { %>
  Working on <%= f.ellipsis(plugins.wakatime.projects[0]?.name, {length:16}) %>
    <% } %>
    <% if ((plugins.wakatime.sections.includes("languages"))&&(plugins.wakatime.languages?.length)) { %>
  Mostly coding in <%= plugins.wakatime.languages[0]?.name %>
    <% } %>
    <% // right %>
    <% if (plugins.wakatime.sections.includes("time")) { %>
  ~<%= f(Math.ceil(plugins.wakatime.time.daily)) %> hour<%= s(plugins.wakatime.time.daily) %> of coding per day
    <% } %>
    <% if ((plugins.wakatime.sections.includes("editors"))&&(plugins.wakatime.editors?.length)) { %>
  Coding with <%= plugins.wakatime.editors[0]?.name %>
    <% } %>
    <% if ((plugins.wakatime.sections.includes("os"))&&(plugins.wakatime.os?.length)) { %>
  Using <%= plugins.wakatime.os[0]?.name %>
    <% } %>

    <% { const sections = plugins.wakatime.sections.filter(x => /-graphs$/.test(x)).map(x => x.replace(/-graphs$/, "")), slots = 2 + large %>
    <% for (let i = 0; i < sections.length; i+=slots) { %>
        <% for (let j = 0; j < slots; j++) { const key = sections[i+j] ; const section = plugins.wakatime[key] ; if (!key) continue %>
  **<%= {languages:"Language activity", projects:"Projects activity", editors:"Code editors", os:"Operating systems"}[key] %>**
          <% if (section?.length) { %>
            <% for (const {name, percent, total} of section) { %>
              <% let string = name %>
              <% if (name.length > 25) { string = name.slice(0, 22) + "..." } %>
              <% string = " " ; for (let k = 0; k < 25 - name.length; k++) { string += " " } %>
              <% string += "~ " ; if (total > (60 * 60)) { string += f(Math.ceil(total / (60 * 60))) + " hour" + s(total / (60 * 60)) + " "; } %>
              <% string += f(Math.ceil(total % (60 * 60) / 60)) + " min" + s(total % (60 * 60) / 60); %>
  <%= string %>
            <% } %>
          <% } else { %>
            No WakaTime activity
          <% } %>
        <% } %>
    <% }} %>
  <% } %>
<% } %>