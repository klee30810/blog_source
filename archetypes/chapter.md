+++
title = "{{ replace .Name "-" " " | title }}"
menuTitle = "menu title"
date = {{ .Date }}
weight = 1000
chapter = true
pre = "<b>X. </b>"

+++

### Chapter X

# Some Chapter title

Lorem Ipsum.

{{% children depth="1" showhidden="true" %}}
