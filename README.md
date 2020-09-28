
<!-- README.md is generated from README.Rmd. Please edit that file -->

generate-mvt-by-sf-test
=======================

    library(sf)
    #> Linking to GEOS 3.8.0, GDAL 3.0.4, PROJ 6.3.2

    data_dir <- here::here("data")

    point <- st_sfc(
      st_point(c(35.6809591, 139.7673068)),
      st_point(c(35.6598003, 139.7023894)),
      crs = 4326
    )

    linestring <- st_sfc(
      st_linestring(rbind(c(135, 40), c(142, 35))),
      st_linestring(rbind(c(135, 36), c(141, 39))),
      crs = 4326
    )

    polygon <- st_sfc(
      st_polygon(list(rbind(c(135, 35), c(138, 40), c(141, 38), c(135, 35)))),
      st_polygon(list(rbind(c(135, 35), c(141, 37), c(138, 34), c(135, 35)))),
      crs = 4326
    )

    unlink(data_dir, recursive = TRUE)
    dir.create(data_dir)

    write_mvt <- function(x) {
      name <- deparse(substitute(x))
      write_sf(
        st_sf(geometry = st_transform(x, 3857)),
        dsn = file.path(data_dir, name),
        layer = name,
        driver = "MVT",
        dataset_options = c("MINZOOM=4", "MAXZOOM=9", "TILE_EXTENSION=mvt", "COMPRESS=NO"),
        append = TRUE
      )
    }

    write_mvt(point)
    write_mvt(linestring)
    write_mvt(polygon)
