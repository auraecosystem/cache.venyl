# remove localstatedir -- compiled VCL and slab files will still be here.
post_remove() {
  rm -rf var/lib/vinyl-cache
}

post_install() {
  if groups vcache | grep -q varnish
  then
    echo "Project Varnish has renamed to Vinyl Cache"
    echo "You still have \`varnish' group remaining"
    echo "See https://vinyl-cache.org/docs/9.0/whats-new/upgrading-9.0.html for migration"
  fi
}
