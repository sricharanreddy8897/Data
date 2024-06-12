@Service
public class ApprovalFlowService {

    @Autowired
    private LoginUser loginUser;

    @Autowired
    private NFCFormRepository nfcFormRepository;

    @Autowired
    private TableFormRepository tableFormRepository;

    public ResponseEntity<List<GetApprovalFlowResponse>> getApprovalRecords(GetApprovalReq req) {
        // Determine content type and subtypes
        boolean fetchNFCForms = req.getContentType().isEmpty() || req.getContentType().contains("ASSET");
        boolean fetchTableForms = req.getContentType().isEmpty() || req.getContentType().contains("TABLE");

        // Pagination variables
        int itemsPerPage = req.getItemsPerPage();
        int page = req.getPage();
        int offset = itemsPerPage * (page - 1);

        // Lists to hold results
        List<GetApprovalFlowResponse> approvalFlowResponses = new ArrayList<>();

        // Logic based on user role
        if (loginUser.isAssetManager()) {
            // Case: Asset Manager - fetch forms owned by the user (review managers)
            if (fetchNFCForms) {
                List<NFCForm> nfcForms = nfcFormRepository.findByReviewManagersContains(loginUser);
                // Process NFCForm results
                approvalFlowResponses.addAll(convertNFCFormsToResponse(nfcForms));
            }
            if (fetchTableForms) {
                List<TableForm> tableForms = tableFormRepository.findByReviewManagersContains(loginUser);
                // Process TableForm results
                approvalFlowResponses.addAll(convertTableFormsToResponse(tableForms));
            }
        } else if (loginUser.isAssetSpecialist()) {
            // Case: Asset Specialist - fetch forms assigned to the user (specialists)
            if (fetchNFCForms) {
                List<NFCForm> nfcForms = nfcFormRepository.findBySpecialistsContains(loginUser);
                // Process NFCForm results
                approvalFlowResponses.addAll(convertNFCFormsToResponse(nfcForms));
            }
            if (fetchTableForms) {
                List<TableForm> tableForms = tableFormRepository.findBySpecialistsContains(loginUser);
                // Process TableForm results
                approvalFlowResponses.addAll(convertTableFormsToResponse(tableForms));
            }
        }

        // Pagination logic
        int totalCount = approvalFlowResponses.size();
        int totalPages = (int) Math.ceil((double) totalCount / itemsPerPage);

        int fromIndex = offset;
        int toIndex = Math.min(offset + itemsPerPage, totalCount);
        if (fromIndex <= toIndex && fromIndex >= 0 && toIndex >= 0) {
            approvalFlowResponses = approvalFlowResponses.subList(fromIndex, toIndex);
        } else {
            approvalFlowResponses = Collections.emptyList();
        }

        // Prepare response entity
        HttpHeaders headers = new HttpHeaders();
        headers.add("X-Total-Count", String.valueOf(totalCount));
        headers.add("X-Total-Pages", String.valueOf(totalPages));

        return new ResponseEntity<>(approvalFlowResponses, headers, HttpStatus.OK);
    }

    private List<GetApprovalFlowResponse> convertNFCFormsToResponse(List<NFCForm> nfcForms) {
        return nfcForms.stream()
                .map(this::convertNFCFormToResponse)
                .collect(Collectors.toList());
    }

    private GetApprovalFlowResponse convertNFCFormToResponse(NFCForm nfcForm) {
        GetApprovalFlowResponse response = new GetApprovalFlowResponse();
        // Populate response object from NFCForm entity
        response.setFormId(nfcForm.getNfcFormId());
        response.setContentType(nfcForm.getContentType());
        response.setSubType(nfcForm.getSubType());
        response.setRole(nfcForm.getRole());
        response.setTitle(nfcForm.getTitle());
        // Set other fields as per requirement
        return response;
    }

    private List<GetApprovalFlowResponse> convertTableFormsToResponse(List<TableForm> tableForms) {
        return tableForms.stream()
                .map(this::convertTableFormToResponse)
                .collect(Collectors.toList());
    }

    private GetApprovalFlowResponse convertTableFormToResponse(TableForm tableForm) {
        GetApprovalFlowResponse response = new GetApprovalFlowResponse();
        // Populate response object from TableForm entity
        response.setFormId(tableForm.getTableFormId());
        response.setContentType(tableForm.getContentType());
        response.setSubType(tableForm.getSubType());
        response.setRole(tableForm.getRole());
        response.setTitle(tableForm.getTitle());
        // Set other fields as per requirement
        return response;
    }
}
