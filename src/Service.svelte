<script>
  import { beforeUpdate } from "svelte";
  import { humanTime, filterJSON } from "./utils";

  export let json = {};

  export let removeMe;

  let host = "";
  let address = "";
  let serviceName = "";
  let uptime = "";
  let lastHeartbeat = ""
  let colour = "panel-danger";

  beforeUpdate(function() {
    host = json.host;
    address = json.address;
    serviceName = json.serviceName;
    uptime = humanTime(
      new Date().getTime() - new Date(json.startTime).getTime()
    );
    
    const lastHeartbeatMillis =
      new Date().getTime() - new Date(json._ts).getTime();
    if (lastHeartbeatMillis < 30000) {
      colour = "panel-success";
    } else {
      colour = "panel-danger";
    }

    lastHeartbeat = humanTime(lastHeartbeatMillis)
  });
</script>

<style>
  pre {
    max-height: 100px;
    overflow-y: scroll;
    background-color: white;
    border: none;
    padding: 0;
  }

  .title {
    /* color: red; */
  }
</style>

<div class="col-lg-3 col-md-4 col-sm-4 col-xs-6" style="padding: 5px;">
  <div class="panel {colour}">
    <div class="panel-heading">
      <div class="row">
        <div class="checkbox title">
          <label title="">
            {serviceName}
            <div style="font-size: 0.8em;">
              <div>Listening on {address}</div>
              <div>{host}</div>
              <div>Up for {uptime}</div>
              <div>Last heatbeat {lastHeartbeat} ago</div>
            </div>

          </label>
        </div>
      </div>
    </div>
    <div class="panel-body">
      <pre style="font-size: 0.7em;">
        {JSON.stringify(filterJSON(json), null, 2)}
      </pre>

    </div>
    <div class="panel-footer">
      <div role="toolbar" class="btn-toolbar">
        <button class="btn btn-sm btn-danger" on:click={removeMe(json)}>
          <span class="glyphicon glyphicon-remove" />
        </button>
      </div>
    </div>
  </div>
</div>
