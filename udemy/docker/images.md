when choosing an image, start with the official image

- officials dont contain forward slashes
- images can contain more than one tag

`docker image ls` see tag column

 dockers storage driver adds layers changes to images for reuse of disk space

 if an image is shared and changed for container a, but not for container b
 only container a will store the changes (called copy on write)

 layers do not get image ids with docker history and show up as <missing>

images are made up of file system changes and metadata

each layer is uniquely identified and only stored once on a host (cached) so downloading multiple image
so or same changes uses the same disk resources

## dockerfiles
because of dockers internal caching model, it helps to keep the things you change the most
at the bottome of a docker filer and the things you change the least at the top

ENV is not inherited from the repo you're using FROM directive with
but everything else is

in the root of a folder with Dockerfile, run `docker image build -t <name_of_img> .`
