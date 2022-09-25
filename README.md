# New Theme
Update
### Running

Download and install jekyll using:


```console
$sudo apt-get install ruby-full build-essential zlib1g-dev
echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
gem install jekyll bundler
```

And then execute to install all the gems needed for the theme.:

```console
$ bundle
```

To run the page using docker which is easier and requires less installation run the following in linux:

```

$ docker run -it --rm \
    --volume="$PWD:/srv/jekyll" \
    -p 4000:4000 jekyll/jekyll \
    jekyll serve

```

### Posts 

Adding post will only need few steps for more information continue to the pre-made post by the creators of the them [Click Here](/_posts/2019-08-08-write-a-new-post.md)

### Text and Typography

For more information on styles, images and other:

[Click Here](_posts/2019-08-08-text-and-typography.md)
