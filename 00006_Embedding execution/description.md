There are some - rare - scenarios where you want to run your test or query command in the server environment - that is, not using docker. In those cases, simple change `isolated true` to `isolated false`. And that is all!

Obviously in those cases you will not need a docker image, so you can remove the `worker` directory safely. 
