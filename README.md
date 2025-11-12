# homgromb.github.io
yeahyeah
<head>
	<style>
		textarea.AutoFit {
			resize: none;
			width: 100%;
		}
		td {
			padding: 0 1%;
		}
		div.Centered {
		display: table;
			margin-right: auto;
			margin-left: auto;
		}
		table, th, td {
			border: 1px solid #003366;
			border-collapse: collapse;
		}
	</style>
</head>

<div style="background-color:003366;height:80;color:FFFFFF;font-family:georgia;text-align:center;vertical-align:middle">
	<span style="font-size:36">Arcaeca's Word Generator</span><span style="font-size:24">v.3</span><br>
	© July 2023</div>
<div style="background-color:FFFFFF;height:70;color:003366;font-family:georgia;vertical-align:middle;text-align:center;">
	<span id="ToLangCode" contenteditable="true" style="font-size:16" maxlength="3">language name here</span>
</div>	
<table width="100%">
	<colspan>
		<col width="33%">
		<col width="33%">
		<col>
	</colspan>
	<tr>
		<th colspan="2">Input</th>
		<th>Output</th>
	</tr>
	<tr>
		<td colspan="2">
			<label style="font-size:20;font-weight:800" for="WG_Categories">Subpatterns</label>
			<textarea id="WG_Categories" class="AutoFit">V=ɑ/æ/e/i/y/ɨ/ə/o/u
C=p/p'/b/t/t'/d/t͡s/t͡s'/d͡z/t͡ʃ/t͡ʃ'/d͡ʒ/t͡sʲ/t͡s'ʲ/d͡zʲ/k/k'/g/q/q'/ɢ/t͡ɬ/t͡ɬ'/d͡ɮ/ʔ/ʡ/s/z/ʃ/ʒ/zʲ/sʲ/ɬ/x/χ/ʁ̞/h/ɦ/ħ/ʕ/w/r/l/ɫ/m/n/j
H=h/ɦ/ħ/ʕ
J=m/n/l/r/ɫ
A=([H[J/w]=2/JC/C=5][ɑ/æ/ə/o/e](:))
E=([H[J/w]=2/JC/C=5][æ/e/i/y/ɨ/ə](:))
I=([H[J/w]=2/JC/C=5][e/i/y/u/ɨ/ə](:))
U=([H[J/w]=2/JC/C=5][ɑ/ɨ/ə/o/u](:))
				
			</textarea>
		</td>
		<td rowspan="2">
			<div id="WG_Output" style="font-size:14;tab-size:4;">
			lol
			</div>
		</td>
	</tr>
	<tr>
		<td colspan="2">
			<label style="font-size:20;font-weight:800" for="WG_Pattern">Pattern</label>
			<textarea id="WG_Pattern" class="AutoFit">(H[J/w]=2/C=5)[ɑ/æ/ə/o/e](:)AAA((J)C)=5/(H[J/w]=2/C=5)[æ/e/i/y/ɨ/ə](:)EEE((J)C)/(H[J/w]=2/C=5)[e/i/y/u/ɨ/ə](:)III((J)C)/(H[J/w]=2/C=5)[ɑ/ɨ/ə/o/u](:)UUU((J)C)</textarea>
		</td>
	</tr>
	<tr>
		<td>
			<input type="button" value="GENERATE" style="font-weight:600;font-size:14;float:left" onclick="WG_FetchWords();">
			<br>
			<label for="WG_WordNumber">Number of words</label>
			<input type="number" id="WG_WordNumber" value="1" onchange="g_WG_iNumWords = this.valueAsNumber;">
		</td>
		<td>
			<input type="button" value="Save .awgrules" onclick="WG_SaveRuleset();">
			<input type="button" value="Load .awgrules" onclick="WG_LoadRuleset();">
			<input id='AWG_LoadFile' type='file' accept=".awg, .awgrules" hidden/>
		</td>
		<td>
			<input type="button" value="Copy to clipboard" onclick="WG_CopyToClipboard();">
			<br>Copy as:
			<input type="radio" name="WG_OutputOption" value="List" checked="true">List
			<input type="radio" name="WG_OutputOption" value="Block" checked="true">Block
			<br><div style="display:block" id="Copied"></div>
		</td>
	</tr>
</table>
	
	<!--
	<label style="font-size:20;font-weight:800" for="Subpatterns">Subpatterns</label>
	<textarea id="Subpatterns" class="AutoFit">A=r</textarea>
	<label style="font-size:20;font-weight:800" for="Pattern">Pattern</label>
	<textarea id="Pattern" class="AutoFit">(H[J/w]*2/C*5/)[ɑ/æ/ə/o/e](:)AAA((J)C)*5/(H[J/w]*2/C*5/)[æ/e/i/y/ɨ/ə](:)EEE((J)C)/(H[J/w]*2/C*5/)[e/i/y/u/ɨ/ə](:)III((J)C)/(H[J/w]*2/C*5/)[ɑ/ɨ/ə/o/u](:)UUU((J)C)</textarea>
	-->
	
	
<div id="Log">
	<div id="LogErrors" style="display:none;background-color:de645b;width:100%"></div>
	<div id="LogWarnings" style="display:none;background-color:e6cf73;width:100%"></div>
	<div id="LogOutput"></div>
</div>	

	
<script language="javascript" type="text/javascript">
var doc = document;

var tAutoFit = doc.getElementsByClassName("AutoFit");
for (let i = 0; i < tAutoFit.length; i++) {
  tAutoFit[i].setAttribute("style", "height:" + (tAutoFit[i].scrollHeight) + "px;overflow-y:hidden;");
  tAutoFit[i].addEventListener("input", AutoFit_OnInput, false);
}

function AutoFit_OnInput() {
  AutoFit_Resize(this);
}

function AutoFit_Resize(e) {
	e.style.height = 0;
	e.style.height = (e.scrollHeight + 15) + "px";
}

function AutoFit_ResizeH(e) {
	e.style.width = 0;
	e.style.width = (e.scrollWidth + 20) + "px";
}

function AutoFit_ResizeAll() {
	for (let i = 0; i < tAutoFit.length; i++) {
		AutoFit_Resize(tAutoFit[i]);
	}
}




function PushToWarnings(sWarning) {
	console.log(sWarning);
}

function ClearErrors() {
	// ignore
}

function ClearWarnings() {
	// ignore
}
function PushToLog() {
	// ignore
}
function PushToLog_Debug () {
	// ignore
}
function RefreshLog() {
	// ignore
}

function ForceToLog(sInput) {
	doc.getElementById("LogOutput").innerHTML = doc.getElementById("Log").innerHTML + "<br/>" + sInput;
}

var g_reNesting = /[\[\]\(\)]/;
var g_reContinue = /[\[\]\(\)]|(?<=[^\*])\*/;
var g_reException = /(.*?)\^(.*?)$/;
var g_reOptChance = /^\d+(?:.\d+)?(?=:)$/;
var g_reWeight = /(.*)=([-+]?\d*\.?\d*)$/;
var g_reGemination = /\*(\d*)/;
var g_WG_tCategories = {};
var g_WG_tCategoryTrees = {};
var g_WG_tCategoryPermutationCache = {};
var g_WG_sPattern = "";
var g_WG_tTree = [];
var g_WG_sLastPatternInput = "";
var g_WG_sLastSubpatternInput = "";
var g_WG_iNumWords = doc.getElementById("WG_WordNumber").valueAsNumber;
var g_WG_tOutput = [];
var g_WG_reSubs;
var g_WG_iOutputType = 0;

function WG_BuildTrees() {

	g_WG_tCategoryPermutationCache = {};
	
	//g_WG_tCategoryTrees = {};
	//BuildSubpatternTrees();
	
	WG_ParseCategories();
	
	//sContinue = "[\[\]\(\)\*]|"+Object.keys(g_WG_tCategories).join("|");
	//sContinue = "[\[\]\(\)\*]";
	//g_reContinue = new RegExp(sContinue);
	
	g_WG_tTree = [];
	WG_BuildPatternTree();
	
}

function WG_BuildPatternTree() {
	g_WG_sPattern = doc.getElementById("WG_Pattern").value.replaceAll("/",",");
	

	
	for (let sLabel in g_WG_tCategories) {
		let iIter = 0;
		while ((result = g_WG_reSubs.exec(g_WG_sPattern)) && (iIter < 1000))  {
			PushToLog_Debug("subpattern match in "+sLabel+" = "+g_WG_tCategories[sLabel]+" : Matched "+result);
			let temp = g_WG_sPattern;
			temp = temp.substring(0,result[1].length)+"["+g_WG_tCategories[result[2]]+"]"+temp.substring(result[1].length+result[2].length);
			g_WG_sPattern = temp;
			iIter++;
			g_WG_reSubs.lastIndex = 0;
		}
	}
	
	//WG_TransformCategories();
	
	
	
	let t0 = performance.now();
	g_WG_tTree = [];
	g_WG_tTree = WG_BuildTree_FindSubpatterns(g_WG_sPattern);
	let t1 = performance.now();
	PushToLog("Finished building tree. ("+(t1-t0).toFixed(4)+" milliseconds)");
}

function BuildSubpatternTree(sLabel) {
	g_WG_tCategoryTrees[sLabel] = WG_BuildTree_FindSubpatterns(g_WG_tCategories[sLabel]);
}

/*

function BuildSubpatternTrees() {
	g_WG_tCategories = WG_ParseCategories();
	g_WG_tCategoryTrees = {};
	
	g_WG_reSubs = new RegExp("(.*?)("+Object.keys(g_WG_tCategories).sort(function(a,b){ return b.length - a.length }).join("|")+")");
	for (let sLabel in g_WG_tCategories) {
		let iPrevIndex = 0;
		let iIter = 0;
		while ((result = g_WG_reSubs.exec(g_WG_tCategories[sLabel])) && (iIter < 1000))  {
			console.log("subpattern match in "+sLabel+" = "+g_WG_tCategories[sLabel]+" : Matched "+result);
			let temp = g_WG_tCategories[sLabel];
			temp = temp.substring(0,result[1].length)+"["+g_WG_tCategories[result[2]]+"]"+temp.substring(result[1].length+result[2].length);
			g_WG_tCategories[sLabel] = temp;
			iIter++;
		}
	}
	
	for (let sLabel in g_WG_tCategories) {
		BuildSubpatternTree(sLabel);
	}
}

*/

function WG_ParseCategories() {
	let t0 = performance.now();
	
	g_WG_tCategories = {};
	
	let tParseSubpatterns = doc.getElementById("WG_Categories").value.split("\n");
	//PushToLog_Debug(tParseSubpatterns)
	let iParseCategories = tParseSubpatterns.length;
	//PushToLog_Debug(iParseCategories)
	let iValidCategories = 0;
	let tOut = {}
	for (let i = 0; i < iParseCategories; i++) {
		let sCurrentCategory = tParseSubpatterns[i];
		PushToLog_Debug("sub \""+sCurrentCategory+"\" ("+typeof(sCurrentCategory)+")");
		let iEqIndex = sCurrentCategory.indexOf("=");
		if (iEqIndex > -1) {
			let sCategoryName = sCurrentCategory.substring(0,iEqIndex);
			if (sCategoryName.length > 0) {
				if (!tOut.hasOwnProperty(sCategoryName)) {
					tOut[sCategoryName] = sCurrentCategory.substring(iEqIndex+1).replaceAll("/",",");
					iValidCategories++;
				} else { PushToWarnings("Unusable category \""+sCurrentCategory+"\": identifier \""+sCategoryName+"\" already in use.") }
			} else { PushToWarnings("Invalid category: \""+sCurrentCategory+"\" has no identifier."); }
		} else { PushToWarnings("Invalid category: \""+sCurrentCategory+"\" has no \"=\" delimiter."); }
	}
	
	
	g_WG_reSubs = new RegExp("(.*?)("+Object.keys(tOut).sort(function(a,b){ return b.length - a.length }).join("|")+")", "g");
	let tKeys = Object.keys(tOut);
	for (let sLabel in tOut) {
		console.log("Substituting categories in category "+sLabel+":");
		let iPrevIndex = 0;
		let iIter = 0;
		
		let iArrayIndex = tKeys.indexOf(sLabel);
		let tKeysOther = tKeys.slice(0,iArrayIndex).concat(tKeys.slice(iArrayIndex+1));
		
		let reSubsOther = new RegExp("(.*?)("+tKeysOther.sort(function(a,b){ return b.length - a.length }).join("|")+")", "g");
		console.log("\tRegex "+sLabel+":");
		console.log(reSubsOther);
		while ((result = reSubsOther.exec(tOut[sLabel])) && (iIter < 1000))  {
			PushToLog_Debug("subpattern match in "+sLabel+" = "+tOut[sLabel]+" : Matched "+result);
			let temp = tOut[sLabel];
			temp = temp.substring(0,result[1].length)+"["+tOut[result[2]]+"]"+temp.substring(result[1].length+result[2].length);
			tOut[sLabel] = temp;
			iIter++;
			reSubsOther.lastIndex = 0;
		}
	}
	
	
	
	g_WG_tCategories = tOut;
	
	let t1 = performance.now();
	PushToLog("Parsed "+iValidCategories+" of "+iParseCategories+" categories. ("+(t1-t0).toFixed(4)+" milliseconds)");
	return tOut;
}

function WG_TransformCategory(sCategory) {
	if (!g_WG_tCategoryPermutationCache.hasOwnProperty(sCategory)) {
		g_WG_tCategories[sCategory] = WG_PermuteCategories(g_WG_tCategories[sCategory].replaceAll("/",","));
		g_WG_tCategoryPermutationCache[sCategory] = g_WG_tCategories[sCategory];
	}
}

function WG_TransformCategories() {
	let t0 = performance.now();
	let tOut = {};
	for (let sKey in g_WG_tCategories) {
		WG_TransformCategory(sKey);
	}
	let t1 = performance.now();
	PushToLog("Transformed categories. ("+(t1-t0).toFixed(4)+" milliseconds)");
	return tOut;
}


function WG_WeightedSelection(tItems, tWeightsIn) {
	// since the algorithm below mutates the weight array in-place, we first have to make a shallow copy of italic
	// otherwise repeated calls will cause the final weight to grow exponential from the accumulation of past calls
	let tWeights = tWeightsIn.slice();
	// transform weight table into a discrete cumulative density function
	let len = tWeights.length;
	for (let i = 0; i < len; i++) { tWeights[i] += tWeights[i-1] || 0; }
	// generate a random number
	let rand = Math.random() * tWeights[len - 1]; // Math.random() is always a float between 0.0 and 1.0
	// and then find where in CDF the number falls
	let i = 0;
	for (i; i < len; i++) if (tWeights[i] > rand) break;
	return tItems[i];
}



function WG_GenerateWord() {
	return WG_BuildSegments(WG_ChooseSubpattern(g_WG_tTree), 0);
}

function WG_FetchWords() {
	
	ClearErrors();
	ClearWarnings();
	
	let tOut = [];
	
	/*
	let sCurrentPatternInput = doc.getElementById("WG_Pattern").value;
	if (g_WG_sLastPatternInput != sCurrentPatternInput) {
		g_WG_sLastPatternInput = sCurrentPatternInput;
		WG_BuildPatternTree();
	}
	
	let sCurrentCategoryInput = doc.getElementById("WG_Categories").value;
	if (g_Wg_WG_sLastSubpatternInput != sCurrentCategoryInput) {
		g_Wg_WG_sLastSubpatternInput = sCurrentCategoryInput;
		BuildSubpatternTrees();
	}
	*/
	let sCurrentPatternInput = doc.getElementById("WG_Pattern").value;
	let sCurrentCategoryInput = doc.getElementById("WG_Categories").value;
	if ( (g_WG_sLastPatternInput != sCurrentPatternInput) || (g_Wg_WG_sLastSubpatternInput != sCurrentCategoryInput) ) {
		g_WG_sLastPatternInput = sCurrentPatternInput;
		g_Wg_WG_sLastSubpatternInput = sCurrentCategoryInput;
		WG_BuildTrees();
	}
	
	
	for (let i = 0; i < g_WG_iNumWords; i++) {
		tOut.push(WG_GenerateWord());
	}
	
	g_WG_tOutput = tOut;
	
	doc.getElementById("WG_Output").innerHTML = tOut.join("	");
	
	RefreshLog();
}

function WG_CopyToClipboard() {
	let Dummy = doc.createElement("textarea");
	doc.body.appendChild(Dummy);
	if (doc.getElementsByName("WG_OutputOption")[1].checked) {
		Dummy.value = g_WG_tOutput.join(" ");
	} else {
		Dummy.value = g_WG_tOutput.join("\n");
	}
	Dummy.select();
	doc.execCommand("copy");
    doc.body.removeChild(Dummy);
	doc.getElementById("Copied").innerHTML = "Changed outputs copied to clipboard:<br><span style=\"font-family:monospace;background-color:#EEEEEE\">"+g_WG_tOutput.join(" ")+"</span>";
}





function WG_ChooseSubpattern(tTree) {
	/*
	tItems = tTree[1];
	tWeights = tTree[2];
	*/
	tItems = tTree.Items;
	tWeights = tTree.Weights;
	return WG_WeightedSelection(tItems, tWeights);
}

/*
function WG_BuildSegments(tSegments) {
	let tOut = [];
	console.log("Total segments: "+tSegments.length);
	for (let i = 0; i < tSegments.length; i++) {
		
		let sAdd = "";
		oSegment = tSegments[i];
		console.log(oSegment);
		if (Array.isArray(oSegment[1])) {
			sAdd = WG_BuildSegments(WG_ChooseSubpattern(oSegment));
			tOut.push(sAdd);
		} else if (typeof(oSegment == "string")) {
			sAdd = oSegment;
			//let tAdd = WG_PermuteCategories(sAdd);
			if (oSegment.charAt(0) == "*") {
				console.log("star moment in '"+oSegment+"'");
				console.log("substring: "+oSegment.substring(1));
				let iNum = parseInt(oSegment.substring(1));
				console.log("geminate to "+iNum+" times");
				if (iNum > 1) {
					tExtraCopies = Array(iNum - 1).fill(tOut[tOut.length - 1]);
					tOut = tOut.concat(tExtraCopies);
				}
			} else {
				tOut.push(sAdd);
			}
			
		}
		//tOut.push(sAdd);
	}
	return tOut.join("");
}
*/


function WG_BuildSegments(pSegments, iIndentLevel) {
	console.log("\t".repeat(iIndentLevel)+"-------------------------------------");
	//let [pSegments, tExceptions] = tSegments;
	//if (Array.isArray(pSegments)) {
	if (pSegments.constructor == Object) {
		
		let tSegments = pSegments.Items;
		let tExceptions = pSegments.Exceptions;
		console.log("\t".repeat(iIndentLevel)+"Deciding "+pSegments.Name+"...");
		
		
		console.log("\t".repeat(iIndentLevel)+"pSegments:");
		console.log(pSegments);
		console.log("\t".repeat(iIndentLevel)+"tExceptions:");
		console.log(tExceptions);
		
		let iTries = 0;
		let tOut = [""];
		for (let i = 0; i < tSegments.length; i++) {
			let pSegment = tSegments[i];
			let sBuilt = WG_BuildSegments(WG_ChooseSubpattern(pSegment), iIndentLevel + 1);
			if (sBuilt.charAt(0) == "*") {
				console.log("star moment in '"+sBuilt+"'");
				console.log("substring: "+sBuilt.substring(1));
				let iNum = parseInt(sBuilt.substring(1));
				console.log("geminate to "+iNum+" times");
				if (iNum > 1) {
					tExtraCopies = Array(iNum - 1).fill(tOut[tOut.length - 1]);
					tOut = tOut.concat(tExtraCopies);
				}
			} else {
				tOut.push(sBuilt);
			}
		}
		iTries++;
		let sAdd = tOut.join("");
		console.log("\t".repeat(iIndentLevel)+"Tentative output: "+sAdd);
		
		
		while (tExceptions.includes(sAdd)) {
			console.log("\t".repeat(iIndentLevel)+"Try "+iTries+"...");
			if (iTries >= 100) {
				console.log("\t".repeat(iIndentLevel)+"No match found for segment "+pSegments.Name+" in 100 tries; returning empty string.");
				return "";
			}
			tOut = [""];
			for (let i = 0; i < tSegments.length; i++) {
				let pSegment = tSegments[i];
				//tOut.push(WG_BuildSegments(WG_ChooseSubpattern(pSegment), iIndentLevel + 1));
				let sBuilt = WG_BuildSegments(WG_ChooseSubpattern(pSegment), iIndentLevel + 1);
				if (sBuilt.charAt(0) == "*") {
					console.log("star moment in '"+sBuilt+"'");
					console.log("substring: "+sBuilt.substring(1));
					let iNum = parseInt(sBuilt.substring(1));
					console.log("geminate to "+iNum+" times");
					if (iNum > 1) {
						tExtraCopies = Array(iNum - 1).fill(tOut[tOut.length - 1]);
						tOut = tOut.concat(tExtraCopies);
					}
				} else {
					tOut.push(sBuilt);
				}
			}
			iTries++;
			sAdd = tOut.join("");
			console.log("\t".repeat(iIndentLevel)+"Tentative output: "+sAdd);
		}
		/*
		console.log("\t".repeat(iIndentLevel)+"Presumed output: "+String(sAdd));
		if (sAdd.charAt(0) == "*") {
			console.log("star moment in '"+sAdd+"'");
			console.log("substring: "+sAdd.substring(1));
			let iNum = parseInt(sAdd.substring(1));
			console.log("geminate to "+iNum+" times");
			if (iNum > 1) {
				tExtraCopies = Array(iNum - 1).fill(tOut[tOut.length - 1]);
				tOut = tOut.concat(tExtraCopies);
			}
		}
		*/
		console.log("\t".repeat(iIndentLevel)+"Output: "+tOut.join(""));
		console.log("\t".repeat(iIndentLevel)+"-------------------------------------");
		return tOut.join("");
	} else if (typeof(pSegments == "string")) {
		console.log("\t".repeat(iIndentLevel)+"Output: "+pSegments);
		console.log("\t".repeat(iIndentLevel)+"-------------------------------------");
		return pSegments;
	}
}




function WG_BuildSubpattern(sSub) {
	return WG_BuildSegments(WG_ChooseSubpattern(g_WG_tCategoryTrees[sSub]));
}


function WG_BuildTree_SegmentSubpattern(sIn, iIndentLevel = 0) {
	PushToLog_Debug("\t".repeat(iIndentLevel)+"Segmenting "+sIn);
	let tSegments = [];
	let tOptional = [];
	let tExceptions = [];
	let iNesting = 0;
	let iLastDelimiter = -1;
	
	/*
	let tStars = {};
	while (tResult = g_reGemination.exec(sIn)) {
		tStars[tResult[1].length] = parseInt(tResult[2]);
	}
	*/
	
	/*
	let tResult = g_reException.exec(sIn);
	if (tResult != null) {
		let iCaronIndex = tResult[1].length;
		let sExceptionPattern = tResult[2];
		while (tCategoryResults = g_WG_reSubs.exec(sExceptionPattern)) {
			WG_TransformCategory(tCategoryResults[2]);
		}
		g_WG_reSubs.lastIndex = 0;
		tExceptions = tExceptions.concat(WG_PermuteCategories(sExceptionPattern));
		sIn = sIn.substring(0,iCaronIndex);
	}
	*/
	
	
	for (let i = 0; i < sIn.length; i++){
		
		let sChar = sIn.charAt(i);
		PushToLog_Debug("i = "+i+" ("+sChar+")");
		if (["[","]","(",")"].includes(sChar)) {
			if (["[","("].includes(sChar)) {
				iNesting++;
				if (iNesting == 1) {
					
					if ((i > 0) && (i > (iLastDelimiter + 1))) {
						let sSeg = sIn.substring(iLastDelimiter + 1, i);
						let bOptional = false;
						if (sIn.charAt(iLastDelimiter) == "(") {
							bOptional = true;
						}
						tSegments.push(sSeg);
						tOptional.push(bOptional);
					}
					iLastDelimiter = i;
					//PushToLog_Debug("iLastDelimiter set to "+iLastDelimiter);
					
					
				}
			} else {
				iNesting--;
			}
			if (iNesting == 0) {
				let sSeg = sIn.substring(iLastDelimiter + 1, i);
				bOptional = false;
				if (sIn.charAt(iLastDelimiter) == "(") {
					bOptional = true;
				}
				tSegments.push(sSeg);
				iLastDelimiter = i;
				//PushToLog_Debug("iLastDelimiter set to "+iLastDelimiter);
				tOptional.push(bOptional);
			}
		} else {
			if (iNesting == 0) {
				if (sChar == "*") {
				
					if (i > iLastDelimiter + 1) {
						let sSeg = sIn.substring(iLastDelimiter + 1, i);
						tSegments.push(sSeg);
					}
				
					let iNum = 1;
					let sNum = "";
					for (let j = i + 1; j < sIn.length; j++) {
						//PushToLog_Debug("\tJ = "+j);
						let sChar2 = sIn.charAt(j);
						//PushToLog_Debug("\t\t > '"+sChar2+"'");
						if (!isNaN(sChar2)) {
							sNum = sNum + sChar2;
						} else {
							//PushToLog_Debug("\t\t not a number, stopping search");
							break;
						}
					}
					if (sNum.length > 0) {
						iNum = parseInt(sNum);
					}
					let iNewDelimiter =  i+sNum.length;
					sSeg = sIn.substring(i, iNewDelimiter + 1);
					iLastDelimiter = iNewDelimiter;
					//PushToLog_Debug("iLastDelimiter set to "+iLastDelimiter);
					tSegments.push(sSeg);
					tOptional.push(false);
				} else if (sChar == "^") {
					console.log("Exception in pattern \""+sIn+"\"");
					let sSeg = sIn.substring(iLastDelimiter + 1, i);
					tSegments.push(sSeg);
					tOptional.push(false);
				
				
					let sExceptionPattern = sIn.substring(i+1);
					while (tCategoryResults = g_WG_reSubs.exec(sExceptionPattern)) {
						WG_TransformCategory(tCategoryResults[2]);
					}
					g_WG_reSubs.lastIndex = 0;
					//tExceptions = tExceptions.concat(WG_PermuteCategories(sExceptionPattern));
					tExceptions = WG_PermuteCategories(sExceptionPattern);
					for (let j = 0; j < tExceptions.length; j++) {
						let sExc = tExceptions[j];
						while (tExcResult = /(.*?)\*(\d+?)/.exec(sExc)) {
							let iIndex = tExcResult[1].length; // first capture group does not include * itself
							let iNum = parseInt(tExcResult[2]);
							sExc = sExc.substring(0,iIndex).repeat(iNum) + sExc.substring(iIndex + 1 + tExcResult[2].length);
						}
						tExceptions[j] = sExc;
					}
					//sIn = sIn.substring(0,iCaronIndex);
					
					iLastDelimiter = sIn.length;
					i = sIn.length;
					
				}
			}
		}
				
		
		if ((i >= sIn.length - 1) && (iLastDelimiter < i)) {
			let sSeg = sIn.substring(iLastDelimiter + 1);
			tSegments.push(sSeg);
			tOptional.push(false);
		}
	}
	
	let tOut = [];
	for (let i = 0; i < tSegments.length; i++){
		console.log("\t".repeat(iIndentLevel+1)+"↳ "+tSegments[i]);
		tOut.push(WG_BuildTree_FindSubpatterns(tSegments[i], tOptional[i], iIndentLevel+2));
	}
	
	
	return { "Type": "Segments", "Name": sIn, "Items": tOut, "Exceptions": tExceptions };
	
	
}

function WG_BuildTree_FindSubpatterns(sIn, bOptional, iIndentLevel = 0) {
	PushToLog_Debug("\t".repeat(iIndentLevel)+"Subpatterning "+sIn);
	let tPatterns = [];
	let tWeights = [];
	let iNesting = 0;
	let iLastDelimiter = -1;
	
	
	let iChance = 50;
	if (bOptional) {
		let iPctIndex = sIn.lastIndexOf("%");
		if (iPctIndex > -1) {
			iChance = parseFloat(sIn.substring(iPctIndex + 1));
			if (iChance >= 100) {
				bOptional = false;
			}
			sIn = sIn.substring(0,iPctIndex);
		}
	}
	/*
	if (bOptional) {
		
		let sSub = sIn;
		let iChance = 50;
		if (g_reOptChance.test(sSub)) {
			let iPctIndex = sSub.lastIndexOf("%");
			iChance = parseFloat(sSub.substring(iPctIndex + 1));
			sSub = sSub.substring(0,iPctIndex);
		}
		// if iChance = 100, then this will throw a divide-by-zero error
		// to preempt that, we simply do this function call over again, but as non-optional
		// and if iChance > 100, then that... just doesn't make sense anyway
		if (iChance >= 100) {
			return WG_BuildTree_FindSubpatterns(sSub, false, iIndentLevel + 1); // sSub instead of sIn is intentional
		} else {
			// let iChance = x and iWeight = y.
			// then you can prove this is the correct Chance > Weight conversion by solving:
			// x/100 = y/(y+1)
			// for y given a known x, and given the "empty option" selection weight is always 1.
			let iWeight = iChance/(100-iChance);
		
			tPatterns = [sSub, ""];
			tWeights = [iWeight, 1];
			
		}
		
		
	} else {
	*/
	
	
	for (let i = 0; i < sIn.length; i++){
		switch(sIn.charAt(i)) {
			case ",":
				if (iNesting == 0) {
					
					let sSub = sIn.substring(iLastDelimiter + 1, i);
					//console.log("sSub : "+sSub);
					
					let iWeight = 1;
					/*
					if (g_reWeight.test(sSub)) {
						let iStarIndex = sSub.lastIndexOf("*");
						iWeight = parseFloat(sSub.substring(iStarIndex + 1));
						sSub = sSub.substring(0,iStarIndex);
					}
					*/
					let tResult = g_reWeight.exec(sSub);
					if (tResult != null) {
						let iIndex = tResult[1].length;
						iWeight = parseFloat(tResult[2]);
						sSub = sSub.substring(0,iIndex) + sSub.substring(iIndex+1+tResult[2].length); // +1 because tResult[2] doesn't include the "=" itself
					}
					
					tPatterns.push(sSub);
					tWeights.push(iWeight);
					
					iLastDelimiter = i;
					//console.log("set iLastDelimiter to "+iLastDelimiter);
					
				}
				break;
			case "[":
				iNesting++;
				break;
			case "(":
				iNesting++;
				break;
			case "]":
				iNesting--;
				break;
			case ")":
				iNesting--;
				break;
			//default:
		}
		
	}
	
	let sSub = sIn.substring(iLastDelimiter + 1);
	let iWeight = 1;
	let tResult = g_reWeight.exec(sSub);
	if (tResult != null) {
		let iIndex = tResult[1].length;
		iWeight = parseFloat(tResult[2]);
		sSub = sSub.substring(0,iIndex) + sSub.substring(iIndex+1+tResult[2].length); // +1 because tResult[2] doesn't include the "=" itself
	}
	
	tPatterns.push(sSub);
	tWeights.push(iWeight);
		
	//}
	
	if (bOptional) {
		let iTotalWeight = 0;
		for (let i = 0; i < tWeights.length; i++) {
			iTotalWeight += tWeights[i];
		}
		// we need to figure out some weight K such that the "nothing" option has an iChance percent probability of winning
		// this is solved by:
		// K /(iTotalWeight + K) = iChance/100
		// you may verify for yourself that this rearranges to:
		// K = (iChance * iTotalWeight) / (100 - iChance)
		tPatterns.push("");
		tWeights.push( (iChance*iTotalWeight)/(100-iChance) )
	}
	
	let tPatternsOut = [];
	for (let i = 0; i < tPatterns.length; i++) {
		PushToLog_Debug("\t".repeat(iIndentLevel+1)+"↳ "+tPatterns[i]);
		
		
		
		if (g_reContinue.test(tPatterns[i])) {
			tPatternsOut.push(WG_BuildTree_SegmentSubpattern(tPatterns[i], iIndentLevel+2));
		} else {
			tPatternsOut.push(tPatterns[i]);
		}
		
	}
	
	//return [sIn, tPatternsOut, tWeights];
	return {"Type": "Subpatterns", "Name": sIn, "Items": tPatternsOut, "Weights": tWeights};
}

function WG_SaveRuleset() {
	//GetAppState();
	var pState = {
		//sFromCode: doc.getElementById("FromLangCode").innerHTML,
		//sFromName: doc.getElementById("FromLangName").innerHTML,
		iNumWords: g_WG_iNumWords,
		sPattern: doc.getElementById("WG_Pattern").value,
		sSubpatterns: doc.getElementById("WG_Categories").value,
		iOutputOption: g_WG_iOutputType
	};
  var a = doc.createElement('a');
  a.setAttribute('href', 'data:aserules;charset=utf-8,'+encodeURIComponent(JSON.stringify(pState)));
  a.setAttribute('download', doc.getElementById("ToLangCode").innerHTML+"_gen.awgrules");
  a.click();
}

function WG_LoadRuleset() {
	document.getElementById("AWG_LoadFile").click();
}

document.getElementById('AWG_LoadFile').onchange = function() {
    
	if (typeof window.FileReader !== 'function') {
      PushToErrors("The file API isn't supported on this browser yet.");
      return;
    }
	
	let file = document.getElementById('AWG_LoadFile').files[0];
	
	let fr = new FileReader();
	fr.onload = function(e) {
    // e.target.result should contain the text
		let sState = e.target.result;
		ForceToLog(sState);
		let pState = JSON.parse(sState);
		//doc.getElementById("FromLangCode").innerHTML = pState.sFromCode;
		//doc.getElementById("FromLangName").innerHTML = pState.sFromName;
				
		doc.getElementById("WG_Pattern").value = pState.sPattern;
		doc.getElementById("WG_Categories").value = pState.sSubpatterns;
		g_WG_iNumWords = pState.iNumWords;
		doc.getElementById("WG_WordNumber").value = pState.iNumWords;
		g_WG_iOutputType = pState.iOutputOption;
		doc.getElementsByName("WG_OutputOption")[g_WG_iOutputType].checked = true;
		
		AutoFit_ResizeAll();
	};
	try { fr.readAsText(file);
	} catch(e) { PushToErrors("Could not read file text"); }
 }
 
 
 </script>
