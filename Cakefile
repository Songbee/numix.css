
{readFileSync: read, writeFileSync: write, mkdirSync: to_make_dir} = require "fs"
{join} = require "path"
postgtk = require "postcss-gtk"
autoprefixer = require "autoprefixer-core"

task "build", ->
	
	input_dir = join __dirname, "egtk", "gtk-3.0"
	output_dir = "lib"
	try to_make_dir output_dir
	
	build = (to, {from})->
		input_path = join input_dir, from
		output_path = join output_dir, to
		source_map_output_path = "#{output_path}.map"
		gtk_css = read input_path, "utf8"
		gtk_css += """
			
			
			/* Fix titlebuttons showing border when window isn't in focus */
			.titlebar .titlebutton:backdrop {
				border: 1px solid transparent;
			}
			
			
		"""
		postgtk
			.use autoprefixer browsers: ['> 1%', 'IE 9']
			.process gtk_css, from: input_path, to: output_path, map: inline: yes
			.then (result)->
				write output_path, result.css, "utf8"
				write source_map_output_path, result.map, "utf8" if result.map
			.catch (e)->
				console.error e.stack
	
	build "elementary.css", from: "gtk.css"
	build "elementary-dark.css", from: "gtk-dark.css"

