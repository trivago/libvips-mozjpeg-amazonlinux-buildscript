# A build script for libvips to use mozjpeg on AWS 'AmazonLinux' EC2 instances

## Raison D'être

When building a scalable high-performance stack for image manipulation and compression running on AWS, we encountered the problem that [Sharp](http://sharp.dimens.io/en/stable/), our Node library of choice for high performance image resizing, did not successfully pick up our global installation of [libvips](https://jcupitt.github.io/libvips/) on AWS 'AmazonLinux' EC2 instances, as was stated in their documentation.

Our goal was to make libvips compile with [mozjpeg](https://github.com/mozilla/mozjpeg) for advanced JPEG compression and filesize savings. We easily achieved this on our local dev environment on OSX by installing libvips via homebrew with the '--with-mozjpeg' flag set and having Sharp pick up on this globally installed version of libvips instead of having it download its own version of libvips during installation.

Once we tried to replicate this on 'AmazonLinux' containers in Docker (to simulate AWS EC2 instances running Amazon's custom Linux CentOS fork), this approach failed, however. Neither installation from source (using the various .configure, CFLAGS + CXXFLAGS, environment variables amnd pkgconfig paths) nor a hacky approach of using Linuxbrew could make libvips compile purely with mozjpeg.

Our initial solution was to create a custom fork of Sharp that pulled a custom libvips during installation. However, this would have required us to maintain this fork + libvips source and merge future updates to these libraries manually. Getting a 'mozjpeg' install flag into Sharp is not an option since Sharp's author considers checking for a preinstalled version of libvips a sufficient level of flexibility.

Since we wanted to ensure sufficient flexibility for our solution, too, and not be a hinderance to future updates to Sharp or libvips, our current solution is more elegant: we have modified only the build script of libvips to ensure proper compilation with mozjpeg on AmazonLinux. This way, future updates to the library can be easily merged and libvips will successfully be installed with mozjpeg. Once the build was successful, an installation of the native Sharp will pick up on the preexisting version of libvips and rely on it instead of downloading its own libvips version.

Using this approach, we can now roll out our scalable high-performance stack for image manipulation and compression on AWS AmazonLinux EC2 instances, relying on Sharp plus libvips with mozjpeg support.

## Usage

(tbd)
