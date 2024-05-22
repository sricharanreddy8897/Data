    @Test
    public void testProcessDealerData_NullDealerCostAndGross() {
        dealerCostAndGross = null;

        ResponseEntity<String> response = dealerService.processDealerData(dealerId, dealerCostAndGross);

        assertEquals(HttpStatus.BAD_REQUEST, response.getStatusCode());
        assertEquals("DealerCostAndGross and its LeadSources cannot be null or empty", response.getBody());
    }

    @Test
    public void testProcessDealerData_EmptyLeadSources() {
        dealerCostAndGross.setLeadSources(Arrays.asList());

        ResponseEntity<String> response = dealerService.processDealerData(dealerId, dealerCostAndGross);

        assertEquals(HttpStatus.BAD_REQUEST, response.getStatusCode());
        assertEquals("DealerCostAndGross and its LeadSources cannot be null or empty", response.getBody());
    }

    @Test
    public void testProcessDealerData_NegativeDealerId() {
        dealerId = -1;
        dealerCostAndGross.setLeadSources(Arrays.asList(new Lead(1, 100, 200)));

        ResponseEntity<String> response = dealerService.processDealerData(dealerId, dealerCostAndGross);

        assertEquals(HttpStatus.BAD_REQUEST, response.getStatusCode());
        assertEquals("Dealer ID must be a positive integer", response.getBody());
    }

    @Test
    public void testProcessDealerData_NegativeLeadSourceId() {
        dealerCostAndGross.setLeadSources(Arrays.asList(new Lead(-1, 100, 200)));

        ResponseEntity<String> response = dealerService.processDealerData(dealerId, dealerCostAndGross);

        assertEquals(HttpStatus.BAD_REQUEST, response.getStatusCode());
        assertEquals("LeadSource ID must be a positive integer", response.getBody());
    }

    @Test
    public void testProcessDealerData_NegativeCost() {
        dealerCostAndGross.setLeadSources(Arrays.asList(new Lead(1, -100, 200)));

        ResponseEntity<String> response = dealerService.processDealerData(dealerId, dealerCostAndGross);

        assertEquals(HttpStatus.BAD_REQUEST, response.getStatusCode());
        assertEquals("Cost cannot be negative", response.getBody());
    }

    @Test
    public void testProcessDealerData_NegativeGross() {
        dealerCostAndGross.setLeadSources(Arrays.asList(new Lead(1, 100, -200)));

        ResponseEntity<String> response = dealerService.processDealerData(dealerId, dealerCostAndGross);

        assertEquals(HttpStatus.BAD_REQUEST, response.getStatusCode());
        assertEquals("Gross cannot be negative", response.getBody());
    }

    @Test
    public void testProcessDealerData_DuplicateLeadSourceId() {
        dealerCostAndGross.setLeadSources(Arrays.asList(new Lead(1, 100, 200), new Lead(1, 150, 250)));

        ResponseEntity<String> response = dealerService.processDealerData(dealerId, dealerCostAndGross);

        assertEquals(HttpStatus.BAD_REQUEST, response.getStatusCode());
        assertEquals("Duplicate LeadSource ID found: 1", response.getBody());
    }

    @Test
    public void testProcessDealerData_Success() {
        dealerCostAndGross.setLeadSources(Arrays.asList(new Lead(1, 100, 200), new Lead(2, 150, 250)));

        ResponseEntity<String> response = dealerService.processDealerData(dealerId, dealerCostAndGross);

        assertEquals(HttpStatus.CREATED, response.getStatusCode());
        assertEquals("Cost and Gross for the selected lead sources is saved successfully", response.getBody());
    }
