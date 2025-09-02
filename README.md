function doPost(e) {
  try {
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Sheet1');
    const data = JSON.parse(e.postData.contents);
    
    // Append data to the sheet
    sheet.appendRow([
      data.username,
      data.adherenceType,
      data.startTime,
      data.endTime,
      parseFloat(data.duration)
    ]);
    
    return ContentService.createTextOutput(JSON.stringify({ status: 'success' }))
      .setMimeType(ContentService.MimeType.JSON);
  } catch (error) {
    return ContentService.createTextOutput(JSON.stringify({ status: 'error', message: error.message }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

function doGet(e) {
  try {
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Sheet1');
    const data = sheet.getDataRange().getValues();
    const records = [];
    
    // Skip header row and convert data to JSON
    for (let i = 1; i < data.length; i++) {
      records.push({
        username: data[i][0],
        adherenceType: data[i][1],
        startTime: data[i][2],
        endTime: data[i][3],
        duration: data[i][4]
      });
    }
    
    return ContentService.createTextOutput(JSON.stringify(records))
      .setMimeType(ContentService.MimeType.JSON);
  } catch (error) {
    return ContentService.createTextOutput(JSON.stringify({ status: 'error', message: error.message }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
