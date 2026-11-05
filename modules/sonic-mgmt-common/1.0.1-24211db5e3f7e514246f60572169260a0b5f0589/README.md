# Management-common with shared YANG owners

Registers `sonic-mgmt-common@1.0.1-24211db5e3f7e514246f60572169260a0b5f0589`
from the exact tested [source #3](https://github.com/securely1g/sonic-mgmt-common/pull/3).
The only registry source patch gives its MODULE the immutable source version.

Management-framework and gNMI compile against the public CVL, translib and
OpenConfig bindings and install the owner's runtime TAR. This integration root
and management-common now select the public `openconfig_goyang` and
`openconfig_ygot` modules, sharing one Go type graph. They no longer apply copies
of those two owners' patches. The other existing Go patches, Go 1.25.9/Python 3.13,
import paths, source schemas and Make/vendor inputs remain unchanged.

The ordinary native AMD64/Trixie integration root retains all six build targets
and four tests from registry #165, plus the existing discovery regression from
registry #219: it compiles the separate Go consumer and
public libraries, builds both TARs and checks installed contents, schemas and
20 independent generator runs. `@sonic-mgmt-common//tools/pyang:discovery_test`
adds five cases for real YANG discovery and text/HTML generation, including
prepared SONiC/OpenConfig models and preservation of explicit file order.
Only the two duplicated dependency overrides
are replaced by normal public-module dependencies. It uses no management-common
source override and no DEB producer.

This registration supersedes PR #219 by incorporating deterministic YANG
documentation discovery from landed [source #4](https://github.com/securely1g/sonic-mgmt-common/pull/4),
and selects that fix's original regression in ordinary registry CI. Source
`24211db5` passed [source CI](https://github.com/securely1g/sonic-mgmt-common/actions/runs/42082192635);
registration `8ee0007c` passed [registry CI](https://github.com/securely1g/sonic-bazel-registry/actions/runs/42083630984)
with the original four test targets. The added fifth target still needs normal
registry CI at the follow-up head. These checks do not establish full-image or
fresh-cache reproducibility.
