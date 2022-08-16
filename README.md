# Is SIKE Broken Yet?

<https://issikebrokenyet.github.io>

A knowledge base of most isogeny based cryptosystems and the best attacks on them.


## What? Why?

The summer of 2022 will forever remain impressed in the memories of
cryptographers. Four post-quantum schemes were [selected by NIST for
standardization](https://csrc.nist.gov/News/2022/pqc-candidates-to-be-standardized-and-round-4),
Rainbow was broken [in a weekend on a
laptop](https://eprint.iacr.org/2022/214), and
[SIKE](https://eprint.iacr.org/2022/975)
[was](https://eprint.iacr.org/2022/1026)
[destroyed](https://eprint.iacr.org/2022/1038).

Following the spectacular break on SIKE, a lot of confusion ensued on
how much of isogeny based crypto is left alive, if any at all.  The
purpose of this knowledge base is to keep track of most, if not all,
isogeny based **schemes**, **assumptions** and **attacks**, so to
provide a complete picture of which directions in isogeny based
cryptography are still viable.


## Contributing: Cryptography

The knowledge base consists of three [YAML](https://yaml.org/) files,
easy to read and to edit: [`schemes.yml`](schemes.yml),
[`assumptions.yml`](assumptions.yml) and
[`attacks.yml`](attacks.yml). To add a scheme, assumption or attack,
simply edit those files and create a pull request.

Please adhere to the following data model:

```yaml
### `schemes.yml`

# A short lowercase identifier for the scheme. Must be unique.
sidh:

  # The scheme must have at least one of a short or long form name
  name:
    short: SIDH
    long: Supersingular Isogeny Diffie-Hellman

  # What kind of crypto primitive is this?
  type: Key Exchange
  
  # What security assumptions does it reduce to?
  # Use identifiers in `assumptions.yml`.
  # 
  # If you put more than one assumption, that's understood as an AND:
  # the scheme is broken if any of the assumptions is broken.
  # Try to keep this list minimal, and rely on `assumptions.yml`
  # for reductions between assumptions
  assumptions:
    - sidh

  # What paper(s) describe the scheme?
  # Format is `Label: link`. Use permalinks.
  references:
    JDF11: 'https://doi.org/10.1007/978-3-642-25405-5_2'
    DJP14: 'https://eprint.iacr.org/2011/506'

  # A Markdown field for extra comments. 
  # References in brackets are automatically expanded to links.
  comments: >-
    Fun fact: [JDF11] does not give a name to the scheme.
```

```yaml
### `assumptions.yml`

# A short lowercase identifier for the assumption. Must be unique.
cssi:

  # The assumption must have at least one of a short or long form name
  name:
    short: CSSI
    long: Computational Supersingular Isogeny Problem
    
  # Other names by which the assumption may be known
  aliases:
    - short: SSI-T
      long: Supersingular Isogeny Problem with Torsion point information
    
  # What attacks break the assumption.
  # Use identifiers in `attacks.yml`.
  # 
  # Try to keep this list minimal, and rely on reduces_to
  # for additional attacks that may break weaker assumptions.
  attacks:
    - mitm
    - tani
    - vow
    - castryck-decru

  # What weaker assumptions this assumption reduces to.
  # If any of these is broken, the assumption is broken.
  # Use identifiers in `assumptions.yml`.
  #
  # May contain circular references, in case some assumptions are equivalent.
  reduces_to:
    - cssi-random

  # What paper(s) define the assumption?
  # Format is `Label: link`. Use permalinks.
  references:
    JDF11: 'https://doi.org/10.1007/978-3-642-25405-5_2'

  # A Markdown field for extra comments. 
  # References in brackets are automatically expanded to links.
  comments: >-
    The name CSSI, introduced in [JDF11] is a bit of a misnomer:
    despite the generic sounding name, it is a very specific problem
    related to the security of SIDH.
```

```yaml
### `attacks.yml`

# A short lowercase identifier for the attack. Must be unique.
kuperberg:

  # The assumption must have at least one of a short or long form name
  name:
    long: Kuperberg
    
  # The complexity of the attack.
  # Three values are supported: poly, subexp, exp.
  complexity: subexp
  
  # Whether this is a quantum attack
  quantum: yes
  
  # What paper(s) describe the attack?
  # Format is `Label: link`. Use permalinks.
  references:
    Kup04: "https://arxiv.org/abs/quant-ph/0302112"
    CJS13: "https://doi.org/10.1515/jmc-2012-0016"

  # A Markdown field for extra comments. 
  # References in brackets are automatically expanded to links.
  comment: >-
    [Kup04] describes a generic quantum algorithm to solve the hidden
    shift problem (equivalently, the dihedral hidden subgroup
    problem). [CJS13] uses Kuperberg's algorithm as a subroutine to
    attack the vectorization problem.
```

If all this looks too complicated, but you still want to suggest
adding a scheme, assumption or attack, [create an issue](issues).
Also create an issue if you want to suggest additions to the data
model.


## Contributing: WebDev

The website is generated by a Python script.  We are open on
technologies, if you want to suggest improvements, but we want to keep
it simple.

The build scripts are in [`src/`](src/), the static files are in
[`_site/`](_site/) and the HTML templates in
[`templates/`](templates/).  The easiest way to setup the environment
and to build the site is by using `make`.

```
# Setup the Python virtual environment
make venv

# Make the website
make
```

Or simply run `make`.
