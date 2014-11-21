wandycodes
==========

<?php 
    //refresh the page
	$page = $_SERVER['PHP_SELF'];
	$sec = "15";
	header("Refresh: $sec; url=$page");


	//call operation to their method
	include('operation.php');
	
	//declare the class or create an instance of the class
	$getdata = new OPERATE();

    $filestring = file_get_contents('output.txt');    
    $ar = array();
	$ar = explode(",",$filestring);
	
	$getdata->get_Input($new_array_without_nullsx = array_filter($ar));
	//print_r($new_array_without_nullsx = array_filter($ar));

?>




<?php 


	if(!class_exists('OPERATE')){

		class OPERATE{
		
			public function get_Input($input){
				
				global $incX, $incY , $hold_prev, $hold_aspo;
				$incX =0; $incY = 0;
				$arrayX = array();
				$arrayY = array();
				
				
				for($i = 0; $i < sizeof($input); $i++){
					
					if($hold_prev == 'x'){						
						
						if($hold_aspo == "*"){
							$arrayX[$incX] = $input[$i] + 300;
						}else {
							$arrayX[$incX] = $input[$i];
						}
						
						$incX++;
						
					}else if($hold_prev == 'y'){
						
						if($hold_aspo == "*"){
							$arrayY[$incY] = $input[$i] + 300;
						}else {
							$arrayY[$incY] = $input[$i];
						}
						
						$incY++;
					}
						
					
					 
					if($hold_prev == '*'  || $hold_prev == '#'){
						$hold_aspo = $input[$i-1];
							
					}
					
				 
					$hold_prev = $input[$i];
				}//end of for loop
				
				/* echo "array x = ";
				echo"\n";
				print_r ($arrayX);
				echo"<br>";
				echo"<br>";
				echo"<br>";
				echo "array y = ";
				echo"\n";
				print_r ($arrayY);
				echo"<br>"; 
				 */
				
				$this->sortout($arrayX, $arrayY);
				
			}//end of function


			//get two arrays and return sorted array base y first
		//than base on x if ys are the same
			public function sortout($arrayX, $arrayY){
				
				
				for($i = 0; $i < sizeof($arrayY); $i++){
				
					for($j = $i; $j < sizeof($arrayY); $j++){
				
						if($arrayY[$i]  > $arrayY[$j]){
							$tmpY = $arrayY[$i];
							$arrayY[$i] = $arrayY[$j];
							$arrayY[$j] = $tmpY;
							$tmpX = $arrayX[$i];
							$arrayX[$i] = $arrayX[$j];
							$arrayX[$j] = $tmpX ;
						}				
						if( floor($arrayY[$i])  == floor($arrayY[$j])){
							
							if($arrayX[$i]  > $arrayX[$j]){
								$tmpY = $arrayY[$i];
								$arrayY[$i] = $arrayY[$j];
								$arrayY[$j] = $tmpY;
								$tmpX = $arrayX[$i];
								$arrayX[$i] = $arrayX[$j];
								$arrayX[$j] = $tmpX ;
								
							}				
						}			
					}		
				}
				/*  echo "array x = ";
				echo"\n";
				print_r ($arrayX);
				echo"<br>";
				echo"<br>";
				echo"<br>";
				echo "array y = ";
				echo"\n";
				print_r ($arrayY);
				echo"<br>";   */
				$this->operation($arrayX, $arrayY);
			}//end of the function sortout

			public function operation($arrayX, $arrayY){			
				
				global $seenBefore;
				static $max_num;
				$fileopen = fopen('savetoggle.txt', 'r');
				while(!feof($fileopen)){
					$seenBefore = fgets($fileopen);
				}
				
				
				
				$x0 =  array();
				$y0 = array();
				
				if($seenBefore == 0){
					
					
						$x0 = $arrayX;
						$x1 = fopen("xf.txt", "w");
						$x2 = implode(",",$x0 );
						fwrite($x1, $x2 );
						fclose($x1);
						
						$y0 = $arrayY;
						$y1 = fopen("yf.txt", "w");
						$y2 = implode(",",$y0 );
						fwrite($y1, $y2 );
						fclose($y1);
					
					
				}				
				$xxin = array();
				$xinf = fopen("xf.txt", "r");				
				$xin;
					while(!feof($xinf)){			
						$xin = fgets($xinf);					
					}					
					$xxin = explode(",", $xin);					
					
				$yyin = array();
				$yinf = fopen("yf.txt", "r");				
				$yin;
					while(!feof($yinf)){			
						$yin = fgets($yinf);					
					}					
					$yyin = explode(",", $yin);	
					
					
					//print_r($array_hdcodeseats);	
				
				
				
				$savetoFile = fopen("savetoggle.txt","w");
				fwrite($savetoFile, $seenBefore + 1);
			    
				
				//print_r($array_hdcodeseats);*/
				$this->_compare($xxin,$arrayX,$yyin,$arrayY);
					
			}//end of function operation
			
			public function _compare($xxin,$arrayX,$yyin,$arrayY){
			
				$array_seats = array();				
				for($i = 0; $i < 70; $i++){
					$array_seats[$i] = 0;
				}
				
				
				
				//print_r($array_hdcodeseats);
				
				
				$temp_array = array();				
				global $j,$k;
				
				$j =0;
				do{
					$k =0;
					echo"x"  .  "j -->". $j. "=".$xxin[$j];
					echo("<br>");
					echo"y"  .  "j -->". $j. "=".$yyin[$j];
					echo("<br>");
					echo("<br>");
					echo("<br>");
					echo("<br>");
					echo("<br>");
										
					do{
									
						echo"arrayX"  .  "k -->". $k. "=".$arrayX[$k];
						echo("<br>");
						echo"arrayY"  .  "k -->". $k. "=".$arrayY[$k];
						echo("<br>");
						echo("<br>");
						echo("<br>");						
						if((abs($xxin[$j] - $arrayX[$k]) <= 4) && (abs($yyin[$j] - $arrayY[$k]) <= 5)){
							        echo "equal to";
									echo"<br>";
									$array_seats[$j] = 1;
							        
									break;
						}else{
								
						}
					$k++;
					}while($k < sizeof($arrayX));
				
				$j++;
				}while($j < sizeof($xxin));
				
				/* echo("<br>");
				print_r($array_seats); */
				
				$this->put_to_file($array_seats);
			}//endof function_compare
			
			public function put_to_file($array_seats){				
				
				$_var_string = implode(',', $array_seats);			
				//check if the is existed
				if(file_exists('update.txt')){
					unlink('update.txt');
				} 
				/*echo"<br>";
				echo"<br>";
				echo $_var_string;	
				echo"<br>";
				echo"<br>";*/
				$update_file = fopen('update.txt', 'w') or die('unable to open the file');			
				fwrite($update_file, $_var_string);		
			
			}
			public function Range($array_temp4){
				$tol =5;
				$inc=0;
				$new_array = array();
				for($i =0; $i < sizeof($array_temp4); $i++){
					
					
					for($j = 0; $j < sizeof($array_temp4); $j++){
						
						if(abs($array_temp4[$i] - $array_temp4[$j]) <= $tol){						
							$array_temp4[$j] = $array_temp4[$i];
							
						}
					}
				
				}
				
			return $array_temp4;
			}//end of the function
			
			
			
		}
	}
?>



<?php
	$page = $_SERVER['PHP_SELF'];
	$sec = "10";
	header("Refresh: $sec; url=$page");
	//call the the connection
	//include('connection.php');
	$address = 'localhost';
	$username = 'root';
	$password = '';
	$dbname = 'spotter';
  
	//create the connection_aborted
	$connection = new mysqli($address, $username, $password, $dbname);
  
	//verify the connection
	if($connection-> connect_error){
		die("connection failed: ". $connection_>conect_error);
  
	}
	
	$array_file = array();
	//open the file that holds the data to update the data base64_decode
	$update_file = fopen('update.txt', 'r');
	
	//loop through the file
	
	while(!feof($update_file)){
		$var1 = fgets($update_file);		
		$array_file = explode(',', $var1);
		
	}
	
	
	//updating the database
	 $sql = "UPDATE table2 SET seat1 = $array_file[0], seat2 = $array_file[1] ,
	seat3 = $array_file[2], seat4 = $array_file[3], seat5 = $array_file[4],
	seat6 = $array_file[5], seat7 = $array_file[6], seat8 = $array_file[7],
	seat9 = $array_file[8], seat10 = $array_file[9],

	seat11 = $array_file[10], seat12 = $array_file[11] ,
	seat13 = $array_file[12], seat14 = $array_file[13], seat15 = $array_file[14],
	seat16 = $array_file[15], seat17 = $array_file[16], seat18 = $array_file[17],
	seat19 = $array_file[18], seat20 = $array_file[19],
	
	seat21 = $array_file[20], seat22 = $array_file[21] ,
	seat23 = $array_file[22], seat24 = $array_file[23], seat25 = $array_file[24],
	seat26 = $array_file[25], seat27 = $array_file[26], seat28 = $array_file[27],
	seat29 = $array_file[28], seat30 = $array_file[29],

	seat31 = $array_file[30], seat32 = $array_file[31] ,
	seat33 = $array_file[32], seat34 = $array_file[33], seat35 = $array_file[34],
	seat36 = $array_file[35], seat37 = $array_file[36], seat38 = $array_file[37],
	seat39 = $array_file[38], seat40 = $array_file[39],
	
	seat41 = $array_file[40], seat42 = $array_file[41] ,
	seat43 = $array_file[42], seat44 = $array_file[43], seat45 = $array_file[44],
	seat46 = $array_file[45], seat47 = $array_file[46], seat48 = $array_file[47],
	seat49 = $array_file[48], seat50 = $array_file[49],
	
	seat51 = $array_file[50], seat52 = $array_file[51] ,
	seat53 = $array_file[52], seat54 = $array_file[53], seat55 = $array_file[54],
	seat56 = $array_file[55], seat57 = $array_file[56], seat58 = $array_file[57],
	seat59 = $array_file[58], seat60 = $array_file[59],
	
	seat61 = $array_file[60], seat62 = $array_file[61] ,
	seat63 = $array_file[62], seat64 = $array_file[63], seat65 = $array_file[64],
	seat66 = $array_file[65], seat67 = $array_file[66], seat68 = $array_file[67],
	seat69 = $array_file[68], seat70 = $array_file[69] WHERE id = 1";	
	$connection->query($sql);		
    $connection->close();	
    fclose($update_file);
?>



<html>
    <head>
        <title> Building Information </title>
        
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <link rel="stylesheet" href="http://code.jquery.com/mobile/1.4.2/jquery.mobile-1.4.2.min.css">
        <script src="http://code.jquery.com/jquery-1.10.2.min.js"></script>
        <script src="http://code.jquery.com/mobile/1.4.2/jquery.mobile-1.4.2.min.js"></script>
        <script type="text/javascript" src="jquery.js"></script>
        <script type="text/javascript" src="whcihclassclick.js"></script>-->
        
        <style>
            .ui-btn-a{
                background-color:gold;

            }
            .center-wrapper{
                margin:0 auto;
                text-align: center;
            }
            .ui-head-a{
                text-decoration-color: black;
                background-color: gold;
                text-align:center;

            }
            .ui-block-a
            .ui-block-b
            .ui-block-c
            .ui-block-d
            .ui-block-e{
                text-align: center;
            }
            ol.view-list {
                list-style-type: none;
            }

        </style>

    </head>
	 <div class="seat"></div>
        <div data-role="page" id="buildings" data-theme="b">
            <div data-role="header" class="ui-mini">
                <h1 class="ui-head-a"><span style="color:black;font-weight:bold">CLASSROOM</span></h1>
            </div>
            <div data-role="main" class="ui-content center-wrapper" data-theme="b">
                <div data-role="controlgroup"  class="ui-corner-all ui-btn-a" data-type="horizontal">                    
                    <a href="index.html" class="ui-btn"><span style="color:gold;font-weight:bold">HOME</span></a>
                    <a href="Members" class="ui-btn"><span style="color:whitesmoke;font-weight:bold">MEMBERS</span></a>
                   <!-- <a href="projectinfo" class="ui-btn"><span style="color:whitesmoke;font-weight:bold">PROJECT</span></a>-->                    
                </div><br>
	
	
	
	
	
<!--<div data-role="page" id="clas" data-theme="b">
            <div data-role="header" class="ui-mini">
                <h1 class="ui-head-a"><span style="color:black;font-weight:bold">CLASS INFO</span></h1>
            </div>
            <div data-role="main" class="ui-content center-wrapper" data-theme="b">
                <div data-role="controlgroup"  class="ui-corner-all ui-btn-a" data-type="horizontal">                    
                    <a href="#buildings" class="ui-btn"><span style="color:whitesmoke;font-weight:bold">Home</span></a>
                    <a href="Members" class="ui-btn"><span style="color:whitesmoke;font-weight:bold">Members</span></a>
                    <a href="projectinfo" class="ui-btn"><span style="color:whitesmoke;font-weight:bold">Project</span></a>                    
                </div><br>-->
            </div>
            <div class="ui-grid-d" style="text-align: center" >
                <div class="ui-block-a" style="border:2px solid gold;"><a href="getDatatoShow.php"><span>101</span></a></div>
                <div class="ui-block-b" style="border:2px solid gold;"><a href="getDatatoshow.php"><span>102</span></a></div>
                <div class="ui-block-c" style="border:2px solid gold;"><a href="getDatatoshow.php"><span>103</span></a></div>
                <div class="ui-block-d" style="border:2px solid gold;"><a href="getDatatoshow.php"><span>104</span></a></div>
                <div class="ui-block-e" style="border:2px solid gold;"><a href="#buildings"><span>105</span></a></div>
                <div class="ui-block-a" style="border:2px solid gold;"><a href="#buildings"><span>106</span></a></div>
                <div class="ui-block-b" style="border:2px solid gold;"><a href="#buildings"><span>107</span></a></div>
                <div class="ui-block-c" style="border:2px solid gold;"><a href="#buildings"><span>108</span></a></div>
                <div class="ui-block-d" style="border:2px solid gold;"><a href="#buildings"><span>109</span></a></div>
                <div class="ui-block-e" style="border:2px solid gold;"><a href="#buildings"><span>110</span></a></div>
                <div class="ui-block-a" style="border:2px solid gold;"><a href="#buildings"><span>111</span></a></div>
                <div class="ui-block-b" style="border:2px solid gold;"><a href="#buildings"><span>112</span></a></div>
                <div class="ui-block-c" style="border:2px solid gold;"><a href="#buildings"><span>113</span></a></div>
                <div class="ui-block-d" style="border:2px solid gold;"><a href="#buildings"><span>114</span></a></div>
                <div class="ui-block-e" style="border:2px solid gold;"><a href="#buildings"><span>115</span></a></div>
                <div class="ui-block-a" style="border:2px solid gold;"><a href="#buildings"><span>116</span></a></div>
                <div class="ui-block-b" style="border:2px solid gold;"><a href="#buildings"><span>117</span></a></div>
                <div class="ui-block-c" style="border:2px solid gold;"><a href="#buildings"><span>118</span></a></div>
                <div class="ui-block-d" style="border:2px solid gold;"><a href="#buildings"><span>120</span></a></div>
                <div class="ui-block-e" style="border:2px solid gold;"><a href="#buildings"><span>200</span></a></div>
                <div class="ui-block-a" style="border:2px solid gold;"><a href="#buildings"><span>201</span></a></div>
                <div class="ui-block-b" style="border:2px solid gold;"><a href="#buildings"><span>202</span></a></div>
                <div class="ui-block-c" style="border:2px solid gold;"><a href="#buildings"><span>203</span></a></div>
                <div class="ui-block-d" style="border:2px solid gold;"><a href="#buildings"><span>204</span></a></div>
                <div class="ui-block-e" style="border:2px solid gold;"><a href="#buildings"><span>205</span></a></div>
                <div class="ui-block-a" style="border:2px solid gold;"><a href="#buildings"><span>206</span></a></div>
                <div class="ui-block-b" style="border:2px solid gold;"><a href="#buildings"><span>207</span></a></div>
                <div class="ui-block-c" style="border:2px solid gold;"><a href="#buildings"><span>208</span></a></div>
                <div class="ui-block-d" style="border:2px solid gold;"><a href="#buildings"><span>209</span></a></div>
                <div class="ui-block-e" style="border:2px solid gold;"><a href="#buildings"><span>210</span></a></div>
                <div class="ui-block-a" style="border:2px solid gold;"><a href="#buildings"><span>211</span></a></div>
                <div class="ui-block-b" style="border:2px solid gold;"><a href="#buildings"><span>212</span></a></div>
                <div class="ui-block-c" style="border:2px solid gold;"><a href="#buildings"><span>213</span></a></div>
                <div class="ui-block-d" style="border:2px solid gold;"><a href="#buildings"><span>214</span></a></div>
                <div class="ui-block-e" style="border:2px solid gold;"><a href="#buildings"><span>215</span></a></div>
                <div class="ui-block-a" style="border:2px solid gold;"><a href="#buildings"><span>216</span></a></div>
                <div class="ui-block-b" style="border:2px solid gold;"><a href="#buildings"><span>217</span></a></div>
                <div class="ui-block-c" style="border:2px solid gold;"><a href="#buildings"><span>218</span></a></div>
                <div class="ui-block-d" style="border:2px solid gold;"><a href="#buildings"><span>219</span></a></div>
                <div class="ui-block-e" style="border:2px solid gold;"><a href="#buildings"><span>300</span></a></div>
                <div class="ui-block-a" style="border:2px solid gold;"><a href="#buildings"><span>301</span></a></div>
                <div class="ui-block-b" style="border:2px solid gold;"><a href="#buildings"><span>302</span></a></div>
                <div class="ui-block-c" style="border:2px solid gold;"><a href="#buildings"><span>303</span></a></div>
                <div class="ui-block-d" style="border:2px solid gold;"><a href="#buildings"><span>304</span></a></div>
                <div class="ui-block-e" style="border:2px solid gold;"><a href="#buildings"><span>305</span></a></div>
                <div class="ui-block-a" style="border:2px solid gold;"><a href="#buildings"><span>306</span></a></div>
                <div class="ui-block-b" style="border:2px solid gold;"><a href="#buildings"><span>307</span></a></div>
                <div class="ui-block-c" style="border:2px solid gold;"><a href="#buildings"><span>308</span></a></div>
                <div class="ui-block-d" style="border:2px solid gold;"><a href="#buildings"><span>309</span></a></div>
                <div class="ui-block-e" style="border:2px solid gold;"><a href="#buildings"><span>310</span></a></div>
                <div class="ui-block-a" style="border:2px solid gold;"><a href="#buildings"><span>311</span></a></div>
                <div class="ui-block-b" style="border:2px solid gold;"><a href="#buildings"><span>312</span></a></div>
                <div class="ui-block-c" style="border:2px solid gold;"><a href="#buildings"><span>313</span></a></div>
                <div class="ui-block-d" style="border:2px solid gold;"><a href="#buildings"><span>314</span></a></div>
                <div class="ui-block-e" style="border:2px solid gold;"><a href="#buildings"><span>315</span></a></div>
                <div class="ui-block-a" style="border:2px solid gold;"><a href="#buildings"><span>316</span></a></div>
                <div class="ui-block-b" style="border:2px solid gold;"><a href="#buildings"><span>317</span></a></div>
                <div class="ui-block-c" style="border:2px solid gold;"><a href="#buildings"><span>318</span></a></div>
                <div class="ui-block-d" style="border:2px solid gold;"><a href="#buildings"><span>319</span></a></div>
                <div class="ui-block-e" style="border:2px solid gold;"><a href="#buildings"><span>320</span></a></div>
                <div class="ui-block-a" style="border:2px solid gold;"><a href="#buildings"><span>400</span></a></div>
                <div class="ui-block-b" style="border:2px solid gold;"><a href="#buildings"><span>401</span></a></div>
                <div class="ui-block-c" style="border:2px solid gold;"><a href="#buildings"><span>402</span></a></div>
                <div class="ui-block-d" style="border:2px solid gold;"><a href="#buildings"><span>403</span></a></div>
                <div class="ui-block-e" style="border:2px solid gold;"><a href="#buildings"><span>404</span></a></div>
                <div class="ui-block-a" style="border:2px solid gold;"><a href="#buildings"><span>405</span></a></div>
                <div class="ui-block-b" style="border:2px solid gold;"><a href="#buildings"><span>406</span></a></div>
                <div class="ui-block-c" style="border:2px solid gold;"><a href="#buildings"><span>407</span></a></div>
                <div class="ui-block-d" style="border:2px solid gold;"><a href="#buildings"><span>408</span></a></div>
                <div class="ui-block-e" style="border:2px solid gold;"><a href="#buildings"><span>409</span></a></div>
                <div class="ui-block-a" style="border:2px solid gold;"><a href="#buildings"><span>410</span></a></div>
                <div class="ui-block-b" style="border:2px solid gold;"><a href="#buildings"><span>411</span></a></div>
                <div class="ui-block-c" style="border:2px solid gold;"><a href="#buildings"><span>412</span></a></div>
                <div class="ui-block-d" style="border:2px solid gold;"><a href="#buildings"><span>413</span></a></div>
                <div class="ui-block-e" style="border:2px solid gold;"><a href="#buildings"><span>414</span></a></div>
                <div class="ui-block-a" style="border:2px solid gold;"><a href="#buildings"><span>415</span></a></div>
                <div class="ui-block-b" style="border:2px solid gold;"><a href="#buildings"><span>416</span></a></div>
                <div class="ui-block-c" style="border:2px solid gold;"><a href="#buildings"><span>417</span></a></div>
                <div class="ui-block-d" style="border:2px solid gold;"><a href="#buildings"><span>418</span></a></div>
                <div class="ui-block-e" style="border:2px solid gold;"><a href="#buildings"><span>419</span></a></div>
                <div class="ui-block-a" style="border:2px solid gold;"><a href="#buildings"><span>420</span></a></div>
               
            </div>


            <div data-role="footer" class="ui-mini">         
                <h1>&copy; U.C.F Senior Design Group six Fall 2014</h1> 
            </div>
        </div>
</html>





<!DOCTYPE html>
<html>
    <head>
        <title> Building Information </title>
        <link href="cssfile.css"  type="text/css" rel="stylesheet">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <link rel="stylesheet" href="http://code.jquery.com/mobile/1.4.2/jquery.mobile-1.4.2.min.css">
        <script src="http://code.jquery.com/jquery-1.10.2.min.js"></script>
        <script src="http://code.jquery.com/mobile/1.4.2/jquery.mobile-1.4.2.min.js"></script>
        <script src="jquery.js"></script>
        <style>
            .ui-btn-a{
                background-color:gold;

            }
            .center-wrapper{
                margin:0 auto;
                text-align: center;
            }
            .ui-head-a{
                text-decoration-color: black;
                background-color: gold;
                text-align:center;

            }
            .ui-block-a
            .ui-block-b
            .ui-block-c
            .ui-block-d
            .ui-block-e{
                text-align: center;
            }
            ol.view-list {
                list-style-type: none;
            }

        </style>

    </head>

    <body >
        <div class="seat"></div>
        <div data-role="page" id="buildings" data-theme="b">
            <div data-role="header" class="ui-mini">
                <h1 class="ui-head-a"><span style="color:black;font-weight:bold">BLDG INFO</span></h1>
            </div>
            <div data-role="main" class="ui-content center-wrapper" data-theme="b">
                <div data-role="controlgroup"  class="ui-corner-all ui-btn-a" data-type="horizontal">                    
                    <a href="http://www.ucf.edu" class="ui-btn"><span style="color:gold;font-weight:bold">UCF</span></a>
                    <a href="Members" class="ui-btn"><span style="color:whitesmoke;font-weight:bold">Members</span></a>
                    <a href="projectinfo" class="ui-btn"><span style="color:whitesmoke;font-weight:bold">Project</span></a>                    
                </div><br>
                <div>
	
                    <ol class="view-list" data-role="listview">
                        <li><a href="classes.html">Classroom Building I (CB1) 79</a></li>
                        <li><a href = "classes.html">ADP 	Alpha Delta Pi - Sorority (ADP) 406</a> </li>
                        <li><a href ="classes.html">ATO 	Alpha Tau Omega - Fraternity (ATO) 410</a> </li>
                        <li><a href ="classes.html">AZD 	Alpha Xi Delta - Sorority (AZD) 404</a></li>
                        <li><a href="classes.html">ARF 	Ampac (ARF) 152</a></li>
                        <li><a href="classes.html">LC55 	Aquarius Courtyard #55 (LC55) 55</a></li>
                        <li><a href="#">LC56 	Aquarius Courtyard #56 (LC56) 	56</a></li>
                        <li><a href="#">LC58 	Aquarius Courtyard #58 (LC58) 	58</a></li>
                        <li><a href="#">LC59 	Aquarius Courtyard #59 (LC59) 	59</a></li>
                        <li><a href="#">ARB 	Arboretum (ARB) 	525</a></li>
                        <li><a href="#">VAB 	Art Gallery (VAB) 	51a</a></li>
                        <li><a href="#">BAND 	Band Storage (BAND) 	630</a></li>
                        <li><a href="#">BYC 	Barbara Ying Center (BYC) 	71</a></li>
                        <li><a href="#">JBF 	Baseball Field (JBF) 	basefield</a></li>
                        <li><a href="#">— 	Bennett Research II 	3251</a></li>
                        <li><a href="#">LS01 	Bio Molecular Meeting Portable (LS01) 	601</a></li>
                        <li><a href="#">BIO 	Biological Sciences (BIO) 	20</a></li>
                        <li><a href="#">BTG 	Biological Transgenic Greenhouse (BTG) 	124</a></li>
                        <li><a href="#">BFRC 	Biology Field Research Center (BFRC) 	92</a></li>
                        <li><a href="#">BMRA 	Biomolecular Research Annex (BMRA) 	8114</a></li>
                        <li><a href="#">BPW 	BPW Scholarship House (BPW) 	402</a></li>
                        <li><a href="#">BRE 	Brevard Hall (BRE) 	30</a></li>
                        <li><a href="#">BHNS 	Bright House Networks Stadium (BHNS) 	135</a></li>
                        <li><a href="#">BHC 	Burnett Honors College (BHC) 	95</a></li>
                        <li><a href="#">PRS 	Burnett House (PRS) 	100</a></li>
                        <li><a href="#">COM 	Burnett School of Biomedical Sciences (COM) 	1002</a></li>
                        <li><a href="#">BA1 	Business Administration (BA1) 	45</a></li>
                        <li><a href="#">BA2 	Business Administration II (BA2) 	94</a></li>
                        <li><a href="#">CSEL 	Career Services and Experiential Learning (CSEL) 	140</a></li>
                        <li><a href="#">CEM 	Center for Emerging Media (CEM) 	center-emerging-media</a></li>
                        <li><a href="#">CMMS 	Center for Multilingual Multicultural Studies (CMMS) 	81</a></li>
                        <li><a href="#">ARNA 	CFE Arena (ARNA) 	50</a></li>
                        <li><a href="#">— 	Challenge Course 	u4</a></li>
                        <li><a href="#">CHEM 	Chemistry (CHEM) 	5</a></li>
                        <li><a href="#">G416 	Chi Omega - Sorority (G416) 	416</a></li>
                        <li><a href="#">CIT 	Citrus Hall (CIT) 	85</a></li>
                        <li><a href="#">CB1 	Classroom Building I (CB1) 	79</a></li>
                        <li><a href="#">CL2 	Classroom Building II (CL2) 	classroom-building-ii</a></li>
                        <li><a href="#">CNH 	Colbourn Hall (CNH) 	18</a></li>
                        <li><a href="#">CAH 	College of Arts & Humanities (CAH) 	87</a></li>
                        <li><a href="#">CSB 	College of Sciences (CSB) 	54</a></li>
                        <li><a href="#">CAPS 	Counseling Center (CAPS) 	27</a></li>
                        <li><a href="#">CSC2 	Creative School 1st Grade (CSC2) 	529</a></li>
                        <li><a href="#">CSC1 	Creative School for Children 1 (CSC1) 	24</a></li>
                        <li><a href="#">CSC3 	Creative School Module 2 (CSC3) 	540</a></li>
                        <li><a href="#">CROL 	CREOL (CROL) 	53</a></li>
                        <li><a href="#">DDD 	Delta Delta Delta - Sorority (DDD) 	403</a></li>
                        <li><a href="#">— 	Disc Golf Course 	dgc</a></li>
                        <li><a href="#">UWC 	Duke Energy UCF Welcome Center (UWC) 	96</a></li>
                        <li><a href="#">ECC 	Early Childhood Center (ECC) 	28</a></li>
                        <li><a href="#">ED 	Education (ED) 	21</a></li>
                        <li><a href="#">EOC 	Emergency Operation Center (EOC) 	49</a></li>
                        <li><a href="#">ESTB 	Emergency Services Training Facility (ESTB) 	350</a></li>
                        <li><a href="#">ERL 	Engine Research Lab (ERL) 	76</a></li>
                        <li><a href="#">SEC 	Engineering Field Lab (SEC) 	44</a></li>
                        <li><a href="#">ENG1 	Engineering I (ENG1) 	40</a></li>
                        <li><a href="#">ENG2 	Engineering II (ENG2) 	91</a></li>
                        <li><a href="#">ENP 	Engineering Research Pavilion (ENP) 	319</a></li>
                        <li><a href="#">FSC 	Facilities and Safety (FSC) 	16</a></li>
                        <li><a href="#">ALUM 	Fairwinds Alumni Center (ALUM) 	126</a></li>
                        <li><a href="#">FC-G 	Ferrell Commons - G (FC-G) 	7g</a></li>
                        <li><a href="#">FS65 	Fire Station #62 (FS65) 	351</a></li>
                        <li><a href="#">FLA 	Flagler Hall (FLA) 	86</a></li>
                        <li><a href="#">— 	Football Practice Field 	footpracfld</a></li>
                        <li><a href="#">G415 	Fraternity and Sorority Life (G415) 	415</a></li>
                        <li><a href="#">LC66 	Gemini Courtyard #66 (LC66) 	66</a></li>
                        <li><a href="#">LC67 	Gemini Courtyard #67 (LC67) 	67</a></li>
                        <li><a href="#">LC68 	Gemini Courtyard #68 (LC68) 	68</a></li>
                        <li><a href="#">LC69 	Gemini Courtyard #69 (LC69) 	69</a></li>
                        <li><a href="#">LC70 	Gemini Courtyard #70 (LC70) 	70</a></li>
                        <li><a href="#">HEC 	Harris Corporation Engineering Center (HEC) 	116</a></li>
                        <li><a href="#">HPA1 	Health & Public Affairs I (HPA1) 	80</a></li>
                        <li><a href="#">HPA2 	Health & Public Affairs II (HPA2) 	90</a></li>
                        <li><a href="#">HC 	Health Center & Pharmacy (HC) 	127</a></li>
                        <li><a href="#">H108 	Hercules 108 (H108) 	108</a></li>
                        <li><a href="#">H109 	Hercules 109 (H109) 	109</a></li>
                        <li><a href="#">H110 	Hercules 110 (H110) 	110</a></li>
                        <li><a href="#">H111 	Hercules 111 (H111) 	111</a></li>
                        <li><a href="#">H112 	Hercules 112 (H112) 	112</a></li>
                        <li><a href="#">AV13 	Hercules 113 (AV13) 	113</a></li>
                        <li><a href="#">H114 	Hercules 114 (H114) 	114</a></li>
                        <li><a href="#">H115 	Hercules 115 (H115) 	115</a></li>
                        <li><a href="#">HAB 	Housing Administration (HAB) 	73</a></li>
                        <li><a href="#">HPH 	Howard Phillips Hall (HPH) 	14</a></li>
                        <li><a href="#">— 	IN VIVO Research 	12601</a></li>
                        <li><a href="#">IC 	Innovative Center (IC) 	8112</a></li>
                        <li><a href="#">— 	IST Technology Development Center 	12423</a></li>
                        <li><a href="#">JBF 	Jay Bergman Field - Baseball Stadium (JBF) 	82</a></li>
                        <li><a href="#">JTWC 	John T. Washington Center (JTWC) 	26</a></li>
                        <li><a href="#">G411 	Kappa Alpha Theta - Sorority (G411) 	411</a></li>
                        <li><a href="#">KD 	Kappa Delta - Sorority (KD) 	407</a></li>
                        <li><a href="#">G417 	Kappa Kappa Gamma - Sorority (G417) 	417</a></li>
                        <li><a href="#">KS 	Kappa Sigma - Fraternity (KS) 	413</a></li>
                        <li><a href="#">— 	Knight's Circle 	pl</a></li>
                        <li><a href="#">— 	Knight's Pantry 	knights-pantry</a></li>
                        <li><a href="#">KTRS 	Knightro's (KTRS) 	50b</a></li>
                        <li><a href="#">KP1 	Knights Plaza I (KP1) 	137</a></li>
                        <li><a href="#">LC65 	Lake Claire Community Office and Center (LC65) 	65</a></li>
                        <li><a href="#">— 	Lake Claire Recreational Area 	lake-claire-recreational-area</a></li>
                        <li><a href="#">LAK 	Lake Hall (LAK) 	9</a></li>
                        <li><a href="#">LPS 	Leisure Pool Services (LPS) 	118</a></li>
                        <li><a href="#">LCC 	Libra Community Center (LCC) 	33</a></li>
                        <li><a href="#">LIB 	Library (LIB) 	2</a></li>
                        <li><a href="#">FC-A 	Live Oak Ballroom/Garden Room (FC-A) 	7ab</a></li>
                        <li><a href="#">FC-D 	Marketplace Dining Hall (FC-D) 	7aa</a></li>
                        <li><a href="#">MSB 	Mathematical Sciences Building (MSB) 	12</a></li>
                        <li><a href="#">COM 	Medical Education Building (COM) 	1001</a></li>
                        <li><a href="#">— 	Memory Mall 	u5</a></li>
                        <li><a href="#">— 	Memory Mall 	memorymall</a></li>
                        <li><a href="#">MH 	Millican Hall (MH) 	1</a></li>
                        <li><a href="#">MMAE 	MMAE Facility (MMAE) 	154</a></li>
                        <li><a href="#">MIRC 	Morgridge International Reading Center (MIRC) 	122</a></li>
                        <li><a href="#">TNRP 	Nature Pavilion (TNRP) 	329</a></li>
                        <li><a href="#">N156 	Neptune 156 (N156) 	156</a></li>
                        <li><a href="#">N157 	Neptune 157 (N157) 	157</a></li>
                        <li><a href="#">N158 	Neptune 158 (N158) 	158</a></li>
                        <li><a href="#">NFH 	Nicholson Fieldhouse (NFH) 	128</a></li>
                        <li><a href="#">NSC 	Nicholson School of Communication (NSC) 	75</a></li>
                        <li><a href="#">N101 	Nike 101 (N101) 	101</a></li>
                        <li><a href="#">N102 	Nike 102 (N102) 	102</a></li>
                        <li><a href="#">N103 	Nike 103 (N103) 	103</a></li>
                        <li><a href="#">N104 	Nike 104 (N104) 	104</a></li>
                        <li><a href="#">N105 	Nike 105 (N105) 	105</a></li>
                        <li><a href="#">N106 	Nike 106 (N106) 	106</a></li>
                        <li><a href="#">N107 	Nike 107 (N107) 	107</a></li>
                        <li><a href="#">FC-C 	Office of Student Rights and Responsibilities (FC-C) 	7c</a></li>
                        <li><a href="#">OCPS 	Orange County School System (OCPS) 	546</a></li>
                        <li><a href="#">ORG 	Orange Hall (ORG) 	31</a></li>
                        <li><a href="#">OTC3 	Orlando Tech Center (OTC3) 	8113</a></li>
                        <li><a href="#">OTC5 	Orlando Tech Center - Bldg. 500 (OTC5) 	8120</a></li>
                        <li><a href="#">OTC6 	Orlando Tech Center - Bldg. 600 (OTC6) 	8121</a></li>
                        <li><a href="#">OSH 	Osceola Hall (OSH) 	10</a></li>
                        <li><a href="#">— 	Parking & Transportation Services 	u1</a></li>
                        <li><a href="#">CPS 	Partnership I (CPS) 	8111</a></li>
                        <li><a href="#">P2 	Partnership II (P2) 	8119</a></li>
                        <li><a href="#">P3 	Partnership III (P3) 	8126</a></li>
                        <li><a href="#">LC60 	Pegasus Courtyard #60 (LC60) 	60</a></li>
                        <li><a href="#">LC61 	Pegasus Courtyard #61 (LC61) 	61</a></li>
                        <li><a href="#">LC62 	Pegasus Courtyard #62 (LC62) 	62</a></li>
                        <li><a href="#">LC63 	Pegasus Courtyard #63 (LC63) 	63</a></li>
                        <li><a href="#">LC64 	Pegasus Courtyard #64 (LC64) 	64</a></li>
                        <li><a href="#">PAC 	Performing Arts Center (PAC) 	119</a></li>
                        <li><a href="#">PAC 	Performing Arts Center Music (PAC) 	m119</a></li>
                        <li><a href="#">PAC 	Performing Arts Center Theatre (PAC) 	t119</a></li>
                        <li><a href="#">PSB 	Physical Sciences (PSB) 	121</a></li>
                        <li><a href="#">PBP 	Pi Beta Phi - Sorority (PBP) 	405</a></li>
                        <li><a href="#">POL 	Polk Hall (POL) 	11</a></li>
                        <li><a href="#">PRNT 	Print Shop (PRNT) 	22</a></li>
                        <li><a href="#">PSY 	Psychology Building (PSY) 	99</a></li>
                        <li><a href="#">UPD 	Public Safety Building (Police) (UPD) 	150</a></li>
                        <li><a href="#">RFM 	Rec Field Maintenance (RFM) 	321</a></li>
                        <li><a href="#">RSFR 	Rec Serv Field Restroom (RSFR) 	320</a></li>
                        <li><a href="#">RSP 	Rec Serv Pavilion (RSP) 	318</a></li>
                        <li><a href="#">RSSP 	Rec Serv Soccer Field Pavilion (RSSP) 	317</a></li>
                        <li><a href="#">RWC 	Recreation and Wellness Center (RWC) 	88</a></li>
                        <li><a href="#">RSB 	Recreation Support (RSB) 	25</a></li>
                        <li><a href="#">RC 	Recycling Center (RC) 	327</a></li>
                        <li><a href="#">— 	Reflecting Pond 	pond</a></li>
                        <li><a href="#">KSK 	Reflections Kiosk (KSK) 	310</a></li>
                        <li><a href="#">RH 	Rehearsal Hall (RH) 	19</a></li>
                        <li><a href="#">PVL 	Research Pavilion (PVL) 	8102</a></li>
                        <li><a href="#">LS23 	Retirement Planning & Investments / Athletics (LS23) 	623</a></li>
                        <li><a href="#">OBSV 	Robinson Observatory (OBSV) 	74</a></li>
                        <li><a href="#">— 	Rosen College Apartments 	rosen-college-apartments</a></li>
                        <li><a href="#">— 	RWC Park 	u3</a></li>
                        <li><a href="#">FC-B 	SDES Information Technology (FC-B) 	7b</a></li>
                        <li><a href="#">SEM 	Seminole Hall (SEM) 	32</a></li>
                        <li><a href="#">SX 	Sigma Chi - Fraternity (SX) 	412</a></li>
                        <li><a href="#">STTC 	Simulation, Training and Technology Center (STTC) 	8125</a></li>
                        <li><a href="#">SBDC 	Small Business Development Center (SBDC) 	small-business-development-center</a></li>
                        <li><a href="#">— 	Soccer Practice Field 	socpracfld</a></li>
                        <li><a href="#">SBS 	Softball Stadium (SBS) 	125</a></li>
                        <li><a href="#">FC-E 	SRC Auditorium (FC-E) 	7e</a></li>
                        <li><a href="#">SWRL 	Storm Water Research Lab (SWRL) 	4</a></li>
                        <li><a href="#">FC-F 	Student Disability Services (FC-F) 	7f</a></li>
                        <li><a href="#">STUN 	Student Union (STUN) 	52</a></li>
                        <li><a href="#">SUM 	Sumter Hall (SUM) 	84</a></li>
                        <li><a href="#">TA 	Teaching Academy (TA) 	93</a></li>
                        <li><a href="#">TC1 	Technology Commons I (TC1) 	13</a></li>
                        <li><a href="#">TC2 	Technology Commons II (TC2) 	29</a></li>
                        <li><a href="#">IRIF 	Technology Incubator (IRIF) 	3267</a></li>
                        <li><a href="#">— 	The Venue 	50c</a></li>
                        <li><a href="#">TH 	Theatre (TH) 	6</a></li>
                        <li><a href="#">G409 	Theta Chi - Fraternity (G409) 	409</a></li>
                        <li><a href="#">T1 	Tower I (T1) 	129</a></li>
                        <li><a href="#">T2 	Tower II (T2) 	130</a></li>
                        <li><a href="#">T3 	Tower III (T3) 	132</a></li>
                        <li><a href="#">T4 	Tower IV (T4) 	133</a></li>
                        <li><a href="#">TSS 	Track & Soccer Field (TSS) 	142</a></li>
                        <li><a href="#">— 	Trailer 	501</a></li>
                        <li><a href="#">— 	Trailer 2 	503</a></li>
                        <li><a href="#">— 	Trailer 3 	504</a></li>
                        <li><a href="#">— 	Trailer 4 	505</a></li>
                        <li><a href="#">UCFH 	UCF House (UCFH) 	408</a></li>
                        <li><a href="#">ADRF 	UCF Wild Animal Facility (ADRF) 	117</a></li>
                        <li><a href="#">UTC 	University Tech Center (UTC) 	8110</a></li>
                        <li><a href="#">UTWR 	University Tower (UTWR) 	8118</a></li>
                        <li><a href="#">— 	Veterans Commemorative Site 	veterans-commemorative-site</a></li>
                        <li><a href="#">VPI 	Visitor and Parking Information Center (VPI) 	153</a></li>
                        <li><a href="#">VAB 	Visual Arts Building (VAB) 	51</a></li>
                        <li><a href="#">VOL 	Volusia Hall (VOL) 	8</a></li>
                        <li><a href="#">WT 	Water Tower (WT) 	301v</a></li>
                        <li><a href="#">WD1 	Wayne Densch Center I (WD1) v	38</a></li>
                        <li><a href="#">WD2 	Wayne Densch Center II (WD2) 	39</a></li>
                        <li><a href="#">WDSC 	Wayne Densch Sports Center (WvDSC) 	77</a></li>
                        <li><a href="#">ZTA 	Zeta Tau Alpha - Sorority (ZTA) 	401</a>

                    </ol> 

                </div><br>

                <div data-role="footer" class="ui-mini">         
                    <h1>&copy; U.C.F Senior Design Group six Fall 2014</h1> 
                </div>

            </div>
        </div> 


        <!--second page start-->
        
        

    </body>
</html>



