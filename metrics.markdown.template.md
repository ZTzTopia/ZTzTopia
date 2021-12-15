<%- await include(`partials/activity.ejs`) %>

<%_ if (plugins.wakatime) { _%>
**⏰ WakaTime <%= plugins.wakatime?.days ? `(over last ${{7:"week", 30:"month", 180:"6 months", 365:"year"}[plugins.wakatime.days]})` : "" %>**
  <%_ if (plugins.wakatime.error) { _%>
    <%= plugins.wakatime.error.message %>
  <%_ } else { _%>
    <%_ // left _%>
    <%_ if (plugins.wakatime.sections.includes("time")) { _%>
  ~<%= f(Math.ceil(plugins.wakatime.time.total)) %> coding hour<%= s(plugins.wakatime.time.total) %> recorded
    <%_ } _%>
    <%_ if ((plugins.wakatime.sections.includes("projects"))&&(plugins.wakatime.projects?.length)) { _%>
  Working on <%= f.ellipsis(plugins.wakatime.projects[0]?.name, {length:16}) %>
    <%_ } _%>
    <%_ if ((plugins.wakatime.sections.includes("languages"))&&(plugins.wakatime.languages?.length)) { _%>
  Mostly coding in <%= plugins.wakatime.languages[0]?.name %>
    <%_ } _%>
    <%_ // right _%>
    <%_ if (plugins.wakatime.sections.includes("time")) { _%>
  ~<%= f(Math.ceil(plugins.wakatime.time.daily)) %> hour<%= s(plugins.wakatime.time.daily) %> of coding per day
    <%_ } _%>
    <%_ if ((plugins.wakatime.sections.includes("editors"))&&(plugins.wakatime.editors?.length)) { _%>
  Coding with <%= plugins.wakatime.editors[0]?.name %>
    <%_ } _%>
    <%_ if ((plugins.wakatime.sections.includes("os"))&&(plugins.wakatime.os?.length)) { _%>
  Using <%= plugins.wakatime.os[0]?.name %>
    <%_ } _%>

    <%_ { const sections = plugins.wakatime.sections.filter(x => /-graphs$/.test(x)).map(x => x.replace(/-graphs$/, "")), slots = 2 + large _%>
    <%_ for (let i = 0; i < sections.length; i+=slots) { _%>
        <%_ for (let j = 0; j < slots; j++) { const key = sections[i+j] ; const section = plugins.wakatime[key] ; if (!key) continue _%>
  **<%= {languages:"Language activity", projects:"Projects activity", editors:"Code editors", os:"Operating systems"}[key] %>**
          <%_ if (section?.length) { _%>
            <%_ for (const {name, percent, total} of section) { _%>
              <%_ let string = name _%>
              <%_ if (name.length > 25) { _%>
                <%_ string = name.slice(0, 22) + "..." _%>
              <%_ } _%>
              <%_ string += " " _%> 
              <%_ for (let k = 0; k < 25 - name.length; k++) { _%>
                <%_ string += " " _%>
              <%_ } _%>
              <%_ string += "~ " _%>
              <%_ if (total > (60 * 60)) { _%> 
                <%_ string += f(Math.ceil(total / (60 * 60))) + " hour" + s(total / (60 * 60)) + " " _%>
              <%_ } _%>
              <%_ string += f(Math.ceil(total % (60 * 60) / 60)) + " min" + s(total % (60 * 60) / 60) _%>
              <div class="bar" style="width: <%= percent*80 %>%; background-color: var(--color-calendar-graph-day-L<%= Math.ceil(percent/0.25) %>-bg)"></div>
              <%_ string += " " + Math.round(100 * percent) + "%" _%>
  <%= string %>
            <%_ } _%>
          <%_ } else { _%>
            No WakaTime activity
          <%_ } _%>
        <%_ } _%>
    <%_ }} _%>
  <%_ } _%>
<%_ } _%>