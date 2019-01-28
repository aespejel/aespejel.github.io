---
layout: single
title:  "Primer post!"
categories: jekyll update
---
Y aquí vamos!
Organizando todo y estableciendo el esqueleto de lo que espero que sea el sitio donde volcar todo lo que tengo en la cabeza de vez en cuando. 
Esto no pretende ser un blog corporativo ni nada por el estilo, es un blog personal, donde espero poder recuperar algo de la salud mental que pierdo en mi día a día :D

Queda mucho por hacer, sí, e irá evolucionando poco a poco, pero al menos ya se puede escribir y no es demasiado horrible de leer.

He decidido hacer uso de las [github pages][github-pages] para alojar el blog, un poco por la aparente comodidad que dan, porque ya soy usuario habitual de github y por probar qué tal funcionan. Además, te permiten [poner tu propio dominio][github-pages-custom-domain]!
Usaré [Jekyll][jekyll-website] y el tema minimal mistakes. Es la primera vez que lo uso, tampoco estoy muy familiarizado con el mundo ruby y estos son los pasos que he tenido que dar para poder publicar mi blog:
* Antes de nada, añadiría lo siguiente a mi .bashrc:
{% highlight bash %}
# Ruby exports

export GEM_HOME=$HOME/gems
export PATH=$HOME/gems/bin:$PATH
{% endhighlight %}
Esto lo aprendí por las malas en la web de [troubleshoting][jekyll-troubleshooting] de jekyll y antes de hacerlo bundle trataba de instalar gemas de ruby en algunos directorios de acceso privilegiado y otros no, dándome errores en el proceso del tipo:
```Ignoring nokogiri-1.10.1 because its extensions are not built.```
* Después, instalar una serie de paquetes que no tenía. He probado a hacer esto tanto en Ubuntu como en Fedora y en ambos he tenido que instalar cosas:<br/>

Ubuntu:
{% highlight bash %}
sudo apt install build_essentials ruby ruby-dev libcurl3
gem install bundler
{% endhighlight %}
Fedora:
{% highlight bash %}
sudo dnf install ruby ruby-devel redhat-rpm-config gcc-c++
gem install bundler
{% endhighlight %}

* Tras esto, ya podemos trastear con bundle y jekyll, tal y como dice la [documentación de Github][github-pages-jekyll].

Links útiles!
He leído unas cuantas páginas para llegar a ser capaz de montar todo esto, algunas de las mejores (o que más me han aportado):
* [Este gist][jekyll-gist].
* Este [_config.yml][config-yml], archivo de la propia web del tema elegido (minimal mistakes).
* Esta lista de [dependencias][github-pages-dependencies] de Github.

[github-pages]: https://pages.github.com/
[github-pages-custom-domain]: https://help.github.com/articles/about-supported-custom-domains/
[jekyll-website]: https://jekyllrb.com/
[jekyll-troubleshooting]: https://jekyllrb.com/docs/troubleshooting/
[github-pages-jekyll]: https://help.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/
[jekyll-gist]: https://gist.github.com/widdowquinn/f255783f826f358f5de97186131419a9
[config-yml]: https://github.com/mmistakes/minimal-mistakes/blob/master/_config.yml
[github-pages-dependencies]: https://pages.github.com/versions/
