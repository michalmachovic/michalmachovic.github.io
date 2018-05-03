---
layout: post
title:  "Magento2: Using Grunt"
date:   2017-09-30 07:13:26 +0200
category: Magento2
tags: [magento2,grunt]
---

Magento is reading files from pub/static folder, but you are developing your theme under app/design/frontend, that means each time you want to make change in css file, you need to copy changed file to pub/static folder. This can be automated with grunt.
<br />
Grunt is javascript task runner. So it can copy, minimize, less, etc.... your files.
<br />
<h3>Installation</h3>
This is installation for centOS, it may different for other servers. <br />
We need nodejs first, then npm (node package manager)

`yum install -y gcc-c++ make`<br />
`curl -sL https://rpm.nodesource.com/setup_6.x | sudo -E bash -`<br />
`yum install nodejs`<br />
<br />
Now to your local folder: <br />
`npm install grunt` <br />
`npm install -g grunt-cli`<br />
<br />
Then which grunt plugins you want to use:<br />
`npm install grunt-contrib-copy --save-dev`<br />
`npm install grunt-contrib-watch --save-dev`<br />


<br />
<h3>Using Grunt</h3>

Now create `GruntFile.js` in your folder with content below. This example will copy `local-l.css` file from `app/design` to `pub/static` each time you make change in `app/design`.
<br />

{% highlight js %}
module.exports = function (grunt) {
	grunt.loadNpmTasks('grunt-contrib-copy');
	grunt.loadNpmTasks('grunt-contrib-watch');
	
	const appName = __dirname.substr(__dirname.lastIndexOf('/') + 1);
	
	grunt.initConfig({
		copy: {
		  main: {
		    expand: true,
		    src: 'app/design/frontend/Customgateway/entertainer/web/css/local-l.css',
		    dest: 'pub/static/frontend/Customgateway/entertainer/en_GB/css/',
		    flatten:    true
		  },
		},

		watch: {
			copy: {
				files: ['app/design/frontend/Customgateway/entertainer/web/css/local-l.css'],
				tasks: [ 'copy' ]
			}
		},
	});
	grunt.registerTask('default', [ 'watch' ]);
   }
{% endhighlight %}
