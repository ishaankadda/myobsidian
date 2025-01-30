1. locate the correct python and pytorch version
2. install a `devel` pytorch container
3. install `tensorflow-gpu` with the correct version using `pip`

You can verify the location of cudnn installation using `apt update && apt install mlocate -y && sudo updatedb && locate cudnn.h`

