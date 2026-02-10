 BigDecimal bigDecimal = new BigDecimal("1000000000000000.1234567891234567890");

        ObjectMapper objectMapper = new ObjectMapper();

        // Ensure BigDecimal is always written as plain (no scientific notation)
        objectMapper.enable(JsonGenerator.Feature.WRITE_BIGDECIMAL_AS_PLAIN);

        // Single module, single serializer
        CustomBigDecimalSerializer serializer = new CustomBigDecimalSerializer(); // default US
        SimpleModule module = new SimpleModule();
        module.addSerializer(BigDecimal.class, serializer);
        objectMapper.registerModule(module);

        // --- US format ---
        String jsonUS = objectMapper.writeValueAsString(bigDecimal);
        System.out.println("US: " + jsonUS);

        // --- EURO format dynamically ---
        serializer.setFormatType(CustomBigDecimalSerializer.FormatType.EURO);
        String jsonEU = objectMapper.writeValueAsString(bigDecimal);
        System.out.println("EURO: " + jsonEU);

        // --- Switch back to US ---
        serializer.setFormatType(CustomBigDecimalSerializer.FormatType.US);
        String jsonUS2 = objectMapper.writeValueAsString(bigDecimal);
        System.out.println("US again: " + jsonUS2);

US: "1,000,000,000,000,000.1234567891234567890"
EURO: "1.000.000.000.000.000,1234567891234567890"
US again: "1,000,000,000,000,000.1234567891234567890"
