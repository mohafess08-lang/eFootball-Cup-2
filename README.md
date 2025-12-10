<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>eFootball Cup Versions</title>

<style>
    body {
        background: #111;
        color: white;
        font-family: Arial, sans-serif;
        margin: 0;
        padding: 10px;
    }

    .version-box {
        background: #222;
        margin-bottom: 10px;
        border-radius: 10px;
        overflow: hidden;
    }

    .version-header {
        background: #333;
        padding: 15px;
        font-size: 20px;
        text-align: center;
        cursor: pointer;
    }

    .version-header:hover {
        background: #444;
    }

    .version-content {
        display: none;
        padding: 15px;
        background: #1a1a1a;
    }

    .match, .knockout-box {
        background: #333;
        padding: 10px;
        margin-bottom: 8px;
        border-radius: 6px;
        text-align: center;
        font-size: 16px;
    }

    .section-title {
        margin-top: 15px;
        margin-bottom: 5px;
        font-size: 18px;
        color: gold;
        text-align: center;
    }
</style>

<script>
    function toggle(id) {
        var box = document.getElementById(id);
        box.style.display = (box.style.display === "block") ? "none" : "block";
    }
</script>

</head>

<body>

<!-- VERSION ONE -->
<div class="version-box">
    <div class="version-header" onclick="toggle('v1')">eFootball Cup One</div>
    <div class="version-content" id="v1">

        <div class="section-title">Matches</div>
        <div class="match">Match 1: Soufiane vs Hamza (5 - 5)</div>
        <div class="match">Match 2: Example Match</div>

        <div class="section-title">Knockout Stage</div>
        <div class="knockout-box">Semi Final 1</div>
        <div class="knockout-box">Semi Final 2</div>
        <div class="knockout-box">Final</div>
    </div>
</div>

<!-- VERSION TWO (ALL RESET) -->
<div class="version-box">
    <div class="version-header" onclick="toggle('v2')">eFootball Cup Two</div>
    <div class="version-content" id="v2">

        <div class="section-title">Matches</div>
        <div class="match">Match 1: 0 - 0</div>
        <div class="match">Match 2: 0 - 0</div>

        <div class="section-title">Knockout Stage</div>
        <div class="knockout-box">Semi Final 1: 0 - 0</div>
        <div class="knockout-box">Semi Final 2: 0 - 0</div>
        <div class="knockout-box">Final: 0 - 0</div>
    </div>
</div>

<!-- OLD VERSION (KEEP DATA AS IS) -->
<div class="version-box">
    <div class="version-header" onclick="toggle('v3')">Old Version (Original)</div>
    <div class="version-content" id="v3">

        <div class="section-title">Matches</div>
        <div class="match">Your original match data here</div>

        <div class="section-title">Knockout</div>
        <div class="knockout-box">Your old knockout data</div>

    </div>
</div>

</body>
</html>

