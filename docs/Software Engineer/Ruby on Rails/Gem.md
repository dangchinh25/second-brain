# Gem
- A Gem is basically a library/package/etc in other programming language (sth that other developer wrote and publish for public use)
- All gem will be installed globally (which can be found out using `which <gem_name>`)
- To use a Gem we will need to `require` it (ref: [[Ruby#Library]])

# Bundler
- Bundler is a dependency management tool available as a gem (i.e it is similar to npm/yarn/etc)
- Bundler will read the [[Gem#Gemfile | Gemfile]] for the list of gems need to be installed, fetches metadata from the source provided, resolves the dependencies of each gem, install them and require them while booting
- `bundle install` will also install to the same (or under) directive of other globally installed gem

# Gemfile
- Similar to `package.json` in `Javascript`
- Gemfile is a ruby file where we can specify all the gem that we need for our application and their dependencies
- During the gem installation, [[Gem#Bundler | Bundler]] will search for the Gemfile under the directory mentioned by the environment `BUNDLE_GEMFILE` or at the root of the directory
- Need to run `bundle install` whenever we want to add a new gem to the file

# Gemfile.lock
- Similar to to `package-lock.json` in `Javascript`
- Contain the list of the gems installed along with their version.
- Next time when you run `bundle install`, bundle will read the `Gemfile.lock` and only install the one that are not already stated in there and if run on a new machine it will make sure the exact same version with the `Gemfile.lock` got installed
- Whenever you install a new gem or remove a gem, the `Gemfile.lock` also got updated

# Load Path
- To define a places on computer where the packages are installed
- To check, inside your Ruby program can print it out
```sh
$ puts $LOAD_PATH
```
- This will print out the array that is defined as the `$LOAD_PATH` when Ruby starts your program.
- Each of these lines represents a directory on the computer where Ruby files are stored.
- If you use `require` anywhere in your application, then Ruby will look for a ruby file with the same name in each of these directories