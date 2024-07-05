import org.apache.commons.codec.binary.Base64;
import org.apache.poi.ss.usermodel.*;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;

import java.io.*;
import java.util.Iterator;

public class Base64ToExcel {

    public static void main(String[] args) {
        String base64String = "your_base64_string_here";
        String outputFilePath = "output.xlsx";
        String filteredFilePath = "filtered_output.xlsx";

        try {
            // Step 1: Decode the base64 string to obtain the byte array
            byte[] decodedBytes = Base64.decodeBase64(base64String);

            // Step 2: Write the byte array to a file
            try (FileOutputStream fos = new FileOutputStream(outputFilePath)) {
                fos.write(decodedBytes);
            }

            // Step 3: Read the Excel file and filter rows
            try (FileInputStream fis = new FileInputStream(outputFilePath);
                 Workbook workbook = new XSSFWorkbook(fis)) {

                Sheet sheet = workbook.getSheetAt(0);
                Workbook filteredWorkbook = new XSSFWorkbook();
                Sheet filteredSheet = filteredWorkbook.createSheet();

                int filteredRowNum = 0;
                Iterator<Row> rowIterator = sheet.iterator();

                while (rowIterator.hasNext()) {
                    Row row = rowIterator.next();
                    // Example filter: Copy rows where the value in the first cell is "FilterValue"
                    if (row.getCell(0).getStringCellValue().equals("FilterValue")) {
                        Row filteredRow = filteredSheet.createRow(filteredRowNum++);
                        copyRow(row, filteredRow);
                    }
                }

                // Step 4: Write the filtered rows to a new Excel file
                try (FileOutputStream fos = new FileOutputStream(filteredFilePath)) {
                    filteredWorkbook.write(fos);
                }
                filteredWorkbook.close();
            }

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
