# CostaPy Template - Bare
An unstlyed CostaPy template with Bootstrap

## Usage

- Put the folder in your templates directory
- Add to handler

        import templates.bare.main  as bare

        kwargs["mako"] = {
          "website" : bare.main(directory.page["public"], "home") # page_directory, file_name
        }

- Define a necessary variable on your modules function

        title       = "CostaPy"
        baseurl     = "http://localhost"

        user_roles  = ["guest"]           # A roles that user have
        active_page = "Home"              # Current active page name

        copyright   = "Dita Aji Pratama"  # Copyright on the footer

- Config a topnav menu on your modules function

        menu = [
          {
            "name"  :"Home",
            "href"  :"/",
            "roles" :["guest"]
          },
          {
            "name"  :"About",
            "href"  :"#",
            "roles" :["guest"]
          },
          {
            "name"  :"CostaPy Website",
            "href"  :"https://costapy.ditaajipratama.com",
            "roles" :["guest"]
          }
        ]

- Set a template on your modules function

        from mako.template import Template

        interface = Template(params["mako"]["website"]['template']).render(
          title     = title,
          baseurl   = baseurl,
          topnav    = Template(params["mako"]["website"]['topnav']).render(
            title       = title,
            baseurl     = baseurl,
            menu        = menu,
            user_roles  = user_roles,
            active_page = active_page
          ),
          footer    = Template(params["mako"]["website"]['footer']).render(
            copyright = copyright,
          ),
          container = Template(params["mako"]["website"]['container']).render(
            baseurl   = baseurl,
            greeting  = f"Hello world, welcome to {globalvar.title}"
          )
        )

Here is a complete handler sample

        import cherrypy
        import json
        import config.directory     as directory
        import templates.bare.main  as bare
        import modules.public.home  as public_home

        @cherrypy.tools.accept(media="application/json")
        class handler():
          def index(self, **kwargs):
            kwargs["mako"] = {
              "website" : bare.main(directory.page["public"], "home")
            }
            return public_home.main().html(kwargs)
          index.exposed = True

Here is a complete module sample

        from mako.template import Template

        class main:

          def __init__(self):
            pass

          def html(self, params):

            title = "CostaPy"
            baseurl = "http://localhost"
            menu = [
              {
                "name"  :"Home",
                "href"  :"/",
                "roles" :["guest"]
              },
              {
                "name"  :"About",
                "href"  :"#",
                "roles" :["guest"]
              },
              {
                "name"  :"CostaPy Website",
                "href"  :"https://costapy.ditaajipratama.com",
                "roles" :["guest"]
              }
            ]
            user_roles = ["guest"]
            active_page = "Home"
            copyright = "Dita Aji Pratama"

            interface = Template(params["mako"]["website"]['template']).render(
              title     = title,
              baseurl   = baseurl,
              topnav    = Template(params["mako"]["website"]['topnav']).render(
                title       = title,
                baseurl     = baseurl,
                menu        = menu,
                user_roles  = user_roles,
                active_page = active_page
              ),
              footer    = Template(params["mako"]["website"]['footer']).render(
                copyright = copyright,
              ),
              container = Template(params["mako"]["website"]['container']).render(
                baseurl   = baseurl,
                greeting  = f"Hello world, welcome to {globalvar.title}"
              )
            )

            return interface
