@RestController
@RequestMapping("/dealers")
public class CostAndGrossController {

    @Autowired
    private DealerService dealerService;

    @ApiResponses(value = {
        @ApiResponse(code = 200, message = "successful operation"),
        @ApiResponse(code = 400, message = "Invalid ID supplied"),
        @ApiResponse(code = 404, message = "Cost and Gross not found")
    })
    @GetMapping(value = "/{dealerId}/marketing/leads", produces = MediaType.APPLICATION_JSON_VALUE)
    public ResponseEntity<DealerCostAndGross> getCostAndGrossForLeads(
            @PathVariable("dealerId") Long dealerId,
            @RequestParam(value = "month", required = true) Integer month,
            @RequestParam(value = "year", required = true) Integer year,
            @RequestParam(value = "offset", defaultValue = "0") int offset,
            @RequestParam(value = "limit", defaultValue = "10") int limit) {

        DealerCostAndGross dealer = dealerService.getDealerWithLeads(dealerId, month, year, offset, limit);

        if (dealer != null) {
            return ResponseEntity.ok(dealer);
        } else {
            return ResponseEntity.status(HttpStatus.NOT_FOUND).body(null);
        }
    }
}





@Service
public class DealerService {

    // Assume this is an injected repository or another service that handles data fetching
    @Autowired
    private DealerRepository dealerRepository;

    public DealerCostAndGross getDealerWithLeads(Long dealerId, Integer month, Integer year, int offset, int limit) {
        // Implement the logic to fetch DealerCostAndGross
        // For example:
        return dealerRepository.findCostAndGrossByDealerIdAndMonthAndYear(dealerId, month, year, offset, limit);
    }
}



public class DealerCostAndGross {

    private Long dealerId;
    private List<Lead> leads;
    private Pagination pagination;

    // Getters and setters

    public static class Pagination {
        private int count;
        private int limit;
        private int offset;

        // Getters and setters
    }
}
