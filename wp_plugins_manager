<?php
//Config and select part
include(__DIR__.'/wp-config.php');
$db = new PDO('mysql:host='.DB_HOST.';dbname='.DB_NAME, DB_USER, DB_PASSWORD);
$start_query = 'SELECT option_value FROM '.$table_prefix.'options WHERE option_name="active_plugins"';
$start_select = $db->prepare($start_query);
$start_select->execute();
$start_object = $start_select->fetch(PDO::FETCH_OBJ);
$active_plugins = $start_object->option_value;
$active_plugins_array = unserialize($active_plugins);

//Logic part
function get_plugin_name($plugin_path) {
	$plugin_loader_string = file_get_contents(__DIR__.'/wp-content/plugins/'.$plugin_path);
	$result = preg_match('/Plugin Name:.+/', $plugin_loader_string, $found);
	$str_found = $found[0];
	$trimer_array = explode('Plugin Name:', $str_found);
	$trimed_string = trim($trimer_array[1]);
	return $trimed_string;
}

if(isset($_POST['deactivate'])) {

	unset($active_plugins_array[$_POST['plugin_id']]);
	$update_string = serialize($active_plugins_array);
	$update_query = 'UPDATE '.$table_prefix.'options SET option_value=\''.$update_string.'\' WHERE option_name="active_plugins"';
	$start_update = $db->prepare($update_query);
	$start_update->execute();
	header('Location: '.$_SERVER['REQUEST_URI']);
}

//View part
echo '<table>';
echo '<tr><td>plugin_id</td><td>plugin_name</td></tr>';

foreach ($active_plugins_array as $plugin_id => $plugin_path) {
	
	$plugin_name = get_plugin_name($plugin_path);
	echo '<tr><td>'.$plugin_id.'</td><td>'.$plugin_name.'</td></tr>';
}

echo '</table>';

echo '<form method="post">';
echo '<label>Insert plugin_id: </label><input type="text" name="plugin_id">';
echo '<input type="submit" name="deactivate" value="deactivate">';
echo '</form>';
